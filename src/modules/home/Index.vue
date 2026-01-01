<template>
  <v-row class="pa-10">
    <v-col cols="12" md="6" lg="6">
      <Numeros
        :listNumberSelect="numberSelect"
        :qtdeDezenas="qtdeDezenas"
        @update:qtdeDezenas="qtdeDezenas = $event"
      />
      <Volante
        @numberSelect="insertNumber($event)"
        @novaAposta="insertAposta($event)"
        :maxSelection="qtdeDezenas"
      />
    </v-col>
    <v-col cols="12" md="6" lg="6">
      <Resultado @resultado="getResultado($event)" />
      <Apostas
        :listAposta="list"
        :resultado="resultado"
        @cleanList="list = []"
        @deleteAposta="deleteAposta($event)"
      />
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
      resultado: [],
      qtdeDezenas: 6,
      resultado: [],
      qtdeDezenas: 6
    };
  },
  mounted() {
    const savedList = localStorage.getItem("loterias_apostas");
    if (savedList) {
      try {
        const parsedList = JSON.parse(savedList);
        // Migration for legacy array-only bets
        this.list = parsedList.map(item => {
          if (Array.isArray(item)) {
            return { numbers: item, label: "Bolão Firma" };
          }
          // Apply default label to existing bets if missing
          if (!item.label) {
            item.label = "Bolão Firma";
          }
          return item;
        });
        this.sortBets();
      } catch (e) {
        console.error("Erro ao carregar apostas", e);
      }
    }
  },
  watch: {
    qtdeDezenas(newVal) {
      const validNums = this.numberSelect.filter((n) => n != 0);
      this.insertNumber(validNums);
    },
    list: {
      handler(newVal) {
        localStorage.setItem("loterias_apostas", JSON.stringify(newVal));
      },
      deep: true
    }
  },

  methods: {
    insertNumber(event) {
      this.numberSelect = [...event];
      if (this.numberSelect.length < this.qtdeDezenas) {
        // Calcular a quantidade de posições que precisam ser preenchidas com 0

        const qtdZeros = this.qtdeDezenas - this.numberSelect.length;

        // Preencher as posições restantes com 0
        for (let i = 0; i < qtdZeros; i++) {
          this.numberSelect.push(0);
        }
      }
    },
    insertAposta(event) {
      this.list.push(event);
      this.sortBets();
      this.numberSelect = Array(this.qtdeDezenas).fill("0");
    },
    getResultado(event) {
      this.resultado = [...event];
    },
    deleteAposta(index) {
      this.list.splice(index, 1);
    },
    sortBets() {
      this.list.sort((a, b) => a.numbers.length - b.numbers.length);
    }
  }
};
</script>

<style>
</style>
