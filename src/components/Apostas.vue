<template>
  <v-container>
    <v-row>
      <v-col cols="12" class="pa-0 pb-4">
        <span class="text-h6">Lista de Apostas</span>
      </v-col>
      <v-col cols="12" class="pa-0">
        <v-list class="pa-0">
          <v-list-item v-for="(list, index) in listAposta" :key="index" class="pa-0">
            <v-list-item-content
              v-for="(number, i) in ordenarLista(list)"
              :key="i"
              class="mr-2"
              :class="colorNumber(number) ? 'bgcolor' : ''"
            >[ {{ number }} ]</v-list-item-content>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  props: {
    listAposta: {
      type: Array
    },
    resultado: {
      type: Array
    }
  },

  methods: {
    colorNumber(number) {
      return this.resultado.includes(number);
    },

    compararNumeros(a, b) {
      return a - b;
    },

    ordenarLista(list) {
      let orderList = list.sort(this.compararNumeros);
      let stringsNumeros = orderList.map(number => {
        return number < 10 ? ("0" + number).toString() : number.toString();
      });
      return stringsNumeros;
    }
  }
};
</script>
<style>
.bgcolor {
  background-color: rgb(2, 206, 2);
}
</style>