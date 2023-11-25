<template>
  <v-container>
    <v-row>
      <v-col cols="12" class="pa-0 pb-4">
        <span class="text-h6">Volante</span>
      </v-col>
      <v-col class="pa-2 pl-0" v-for="number in numbers" :key="number" cols="12" md="1" lg="1">
        <v-btn
          :disabled="isButtonDisabled(number)"
          rounded
          :class="{ 'orange-button': isButtonClicked(number), 'initial-button': !isButtonClicked(number) }"
          size="45"
          @click="handleButtonClick(number)"
        >{{ formatNumber(number) }}</v-btn>
      </v-col>
      <v-col cols="12" class="px-0 py-4">
        <v-btn
          @click="addToList"
          color="success"
          :disabled="clickedButtons.length < 6"
        >Adicionar à Lista</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      numbers: Array.from({ length: 60 }, (_, index) => index + 1),
      clickedButtons: [],
      addedLists: []
    };
  },
  methods: {
    handleButtonClick(number) {
      // Se o botão já foi clicado, remover da lista e voltar para a cor original
      if (this.clickedButtons.includes(number)) {
        const index = this.clickedButtons.indexOf(number);
        this.clickedButtons.splice(index, 1);
      } else {
        // Se já foram clicados 6 botões, não fazer nada
        if (this.clickedButtons.length >= 6) {
          return;
        }

        // Se o botão não foi clicado, adicionar à lista
        this.clickedButtons.push(number);
      }

      // Lógica a ser executada ao clicar no botão
      this.$emit("numberSelect", this.clickedButtons);
    },
    isButtonClicked(number) {
      // Verificar se o botão foi clicado
      return this.clickedButtons.includes(number);
    },
    isButtonDisabled(number) {
      // Desativar botões após clicar em 6
      return (
        this.clickedButtons.length >= 6 && !this.clickedButtons.includes(number)
      );
    },
    addToList() {
      // Adicionar a lista de botões clicados à nova lista
      this.addedLists.push([...this.clickedButtons]);
      // Limpar a lista de botões clicados
      this.clickedButtons = [];
      this.$emit("listAposta", this.addedLists);
    },
    formatNumber(number) {
      // Adicionar zero à esquerda se o número for menor que 10
      return number < 10 ? "0" + number : number;
    }
  }
};
</script>

<style>
.initial-button {
  background-color: #eeeeee !important;
}

.orange-button {
  background-color: orange !important;
}
</style>
