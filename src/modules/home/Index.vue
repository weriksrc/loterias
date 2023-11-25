<template>
  <v-row class="pa-10">
    <v-col cols="12" md="6" lg="6">
      <Numeros :listNumberSelect="numberSelect" />
      <Volante @numberSelect="insertNumber($event)" @listAposta="insertAposta($event)" />
    </v-col>
    <v-col cols="12" md="6" lg="6">
      <Resultado @resultado="getResultado($event)" />
      <Apostas :listAposta="list" :resultado="resultado" />
    </v-col>
  </v-row>
</template>

<script>
import Resultado from "../../components/Resultado.vue";
import Numeros from "../../components/Numeros.vue";
import Volante from "../../components/Volante.vue";
import Apostas from "../../components/Apostas.vue";
export default {
  components: {
    Resultado,
    Numeros,
    Volante,
    Apostas
  },

  data() {
    return {
      numberSelect: [],
      list: [],
      resultado: []
    };
  },

  methods: {
    insertNumber(event) {
      this.numberSelect = [...event];
      if (this.numberSelect.length < 6) {
        // Calcular a quantidade de posições que precisam ser preenchidas com 0

        const qtdZeros = 6 - this.numberSelect.length;

        // Preencher as posições restantes com 0
        for (let i = 0; i < qtdZeros; i++) {
          this.numberSelect.push(0);
        }
      }
    },
    insertAposta(event) {
      this.list = [...event];
      this.numberSelect = Array(6).fill("0");
    },
    getResultado(event) {
      this.resultado = [...event];
    }
  }
};
</script>

<style>
</style>
