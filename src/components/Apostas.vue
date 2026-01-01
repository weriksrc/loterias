<template>
  <v-container>
    <v-row>
      <v-col cols="12" class="pa-0 pb-4 d-flex justify-space-between align-center">
        <span class="text-h6">Lista de Apostas</span>
        <v-btn color="error" variant="text" @click="$emit('cleanList')">Limpar</v-btn>
      </v-col>
      <v-col cols="12" class="pa-0">
        <v-list class="pa-0">
          <v-list-item v-for="(bet, index) in listAposta" :key="index" class="pa-0">
            <v-list-item-content class="d-flex align-center flex-wrap">
              <div class="d-flex align-center mr-4" style="min-width: 100px" v-if="bet.label">
                 <span class="text-caption text-grey">Rótulo:</span>
                 <span class="ml-1 font-weight-bold">{{ bet.label }}</span>
              </div>
              <v-btn
                icon
                variant="text"
                color="error"
                size="small"
                class="mr-2"
                @click="$emit('deleteAposta', index)"
              >
                <v-icon>mdi-trash-can-outline</v-icon>
              </v-btn>
              <span
                v-for="(number, i) in ordenarLista(bet.numbers)"
                :key="i"
                class="mr-2"
                :class="colorNumber(number) ? 'bgcolor' : ''"
              >[ {{ number }} ]</span>
              
              <v-chip
                v-if="resultado && resultado.length > 0"
                class="ml-2"
                size="small"
                :color="getCorAcertos(calcularAcertos(bet.numbers))"
                text-color="white"
              >
                {{ calcularAcertos(bet.numbers) }} acertos
              </v-chip>
            </v-list-item-content>
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
    },

    calcularAcertos(bet) {
      if (!this.resultado) return 0;
      // Converte para Number para ignorar zeros à esquerda ( "01" == 1 )
      const resultadoNums = this.resultado.map(Number);
      const betNums = bet.map(Number);
      return betNums.filter(num => resultadoNums.includes(num)).length;
    },

    getCorAcertos(acertos) {
      if (acertos < 4) return 'red';
      if (acertos === 4) return 'orange';
      if (acertos === 5) return 'blue';
      if (acertos === 6) return 'green';
      return 'grey'; // Fallback
    }
  }
};
</script>
<style>
.bgcolor {
  background-color: rgb(2, 206, 2);
}
</style>