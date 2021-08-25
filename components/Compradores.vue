<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>

  </v-card>
</template>

<script>
import axios from "axios";
axios.defaults.headers.common["Content-Type"] = "application/json";

export default {
  name: "Inicio",
  data() {
    return {
      columnas: [
        { text: "ID", value: "id" },
        { text: "COMPRADOR", value: "name" },
        { text: "EDAD", value: "age" },
      ],
      datos: [],
      
    };
  },
  mounted() {
    let vue = this; 
    axios.post("http://localhost:3333/compradores").then(function (response) {
      vue.datos = response.data;
      console.log(vue.datos);
    });
  },
};
</script>