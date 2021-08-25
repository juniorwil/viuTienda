<template>
  <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-card>
      <v-card-title>
        <v-row>
          <v-col cols="6" md="4">
            <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              :return-value.sync="date"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="date"
                  label="Fecha de sincronización de datos"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                  :rules="dateRules"
                  required 
                ></v-text-field>
              </template>
              <v-date-picker v-model="date" no-title scrollable>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="menu = false">
                  Cancel
                </v-btn>
                <v-btn text color="primary" @click="$refs.menu.save(date)">
                  OK
                </v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="6" md="4">
            <v-btn
              color="success"
              class="mr-4"
              :loading="loading"
              @click="consultar()"
            >
              Cargar datos a Neo4j</v-btn
            >
          </v-col>
        </v-row>
      </v-card-title>

      <v-snackbar v-model="snackbar" :timeout="timeout" top color="success">
        {{ text }}

        <template v-slot:action="{ attrs }">
          <v-btn color="white" text v-bind="attrs" @click="snackbar = false">
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </v-card>
  </v-form>
</template>

<script>
import axios from "axios";
axios.defaults.headers.common["Content-Type"] = "application/json";

export default {
  name: "Inicio",
  data() {
    return {
      datos: [],
      loading: false,
      snackbar: false,
      text: "Registros creados de forma correcta",
      timeout: 2000,
      dateRules: [
        v => !!v || 'La fecha es requerida',
        v => (v && v.length <= 10) || 'La fecha debe tener al menos 10 caracteres',
      ],      
    };
  },
  methods: {
    consultar() {
      let vue = this; // Creo unas instancia de la esntancia para que reconocas la variables post
      vue.loading = true;
      const formData = new FormData();
      formData.append("comprador", vue.buscar);
      axios
        .post("http://localhost:3333/cargar", formData)// Apis rest para cargar datos a Neo4j
        .then(function (response) {
          vue.datos = response.data;
          console.log(vue.datos);
          vue.loading = false;
        });
    },
  },
};
</script>