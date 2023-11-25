<template>
  <v-container>
    <v-row>
      <v-col cols="12" class="pa-0 pb-4">
        <span class="text-h4">Resultado</span>
      </v-col>
      <v-col v-for="(field, index) in fields" :key="index" class="pa-0 d-flex align-center" md="2">
        <v-text-field
          v-model="fields[index]"
          type="text"
          variant="outlined"
          inputmode="numeric"
          pattern="[0-9]*"
          @focus="handleFocus(index)"
          @input="handleInput(index)"
          @blur="handleBlur(index)"
          :maxlength="2"
          :ref="`field${index + 1}`"
        />
        <v-icon class="mb-4" :color="index < 5 ? 'black' : 'white'">mdi-minus</v-icon>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      fields: Array(6).fill("00") // Array com 6 strings representando os valores dos campos
      // numberValidationRule: [
      //   value => value < 61 || "Número deve ser menor ou igual a 60"
      // ]
    };
  },
  methods: {
    handleFocus(index) {
      // Limpar o valor quando o campo receber foco
      this.fields[index] = "";
    },
    handleInput(index) {
      // Filtrar para aceitar apenas números
      const inputNumber = this.fields[index].replace(/\D/g, "");
      // Verificar se o número já foi utilizado anteriormente
      if (
        this.fields.slice(0, index).includes(inputNumber) ||
        this.fields.slice(index + 1).includes(inputNumber)
      ) {
        this.fields[index] = ""; // Se o número já foi utilizado, limpar o campo
      } else if (inputNumber > 60) {
        this.fields[index] = "";
      } else {
        this.fields[index] = inputNumber;
      }

      const currentField = this.fields[index];
      if (currentField.length === 2 && index < this.fields.length - 1) {
        this.$nextTick(() => {
          this.$refs[`field${index + 2}`][0].focus();
        });
      }
      this.$emit("resultado", this.fields);
    },
    handleBlur(index) {
      // Restaurar o valor "00" se o campo estiver vazio ao perder o foco
      if (!this.fields[index]) {
        this.fields[index] = "00";
      }
    }
  }
};
</script>

<style>
/* Adicione estilos personalizados aqui, se necessário */
.v-field__input {
  text-align: center !important;
}
.v-field {
  font-size: 2.5rem !important; /* Ajuste o tamanho da fonte conforme necessário */
}
</style>
