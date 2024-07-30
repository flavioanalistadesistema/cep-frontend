<template>
  <div class="container">
    <h1>{{ msg }}</h1>
    <h3>TradeUp Group</h3>
    <form @submit.prevent="buscarCep">
      <input
        type="text"
        v-model="cepInput"
        @input="validaCep"
        placeholder="Digite o CEP"
      />
      <button type="submit" :disabled="isValid">Buscar</button>
    </form>
    <div v-if="validationError" class="dadosError">{{ validationError }}</div>
    <div v-if="dados" class="dadosCep">
      <h2>Dados do CEP:</h2>
      <span><strong>Logradouro:</strong> {{ dados.logradouro }}</span>
      <span><strong>Bairro:</strong> {{ dados.bairro }}</span>
      <span><strong>Cidade:</strong> {{ dados.localidade }}</span>
      <span><strong>Estado:</strong> {{ dados.uf }}</span>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { mapState, mapActions } from "vuex";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      cep: "",
      dados: null,
      isValid: false,
    };
  },
  computed: {
    ...mapState(["dados", "error"]),
  },
  methods: {
    ...mapActions(["buscarCep"]),
    async buscarCep() {
      try {
        this.error = null;
        this.dados = null;
        const response = await axios.get(
          `https://viacep.com.br/ws/${this.cepInput}/json/`
        );
        // Descomente a linha abaixo e comente a linha acima para testar a BACKEND em produção
        // const response = await axios.get(`http://seu-dominio.com/cep_api.php?cep=${cep}`);
        this.dados = response.data;
        this.validationError = null;
      } catch (error) {
        this.error = "Erro ao buscar CEP";
      }
    },
    validaCep() {
      const inputNumber = this.cepInput.replace(/\D/g, "");
      if (inputNumber == "") {
        this.isValid = true;
        this.validationError = null;
        this.validationError =
          "Digite um CEP válido. Deve conter apenas números.";
        return;
      }
      const cepPattern = /^[0-9]{5}-?[0-9]{3}$/;
      if (!cepPattern.test(this.cepInput)) {
        this.validationError = "CEP inválido. Deve conter 8 dígitos numéricos.";
        this.isValid = false;
        return;
      } else {
        this.validationError = null;
        this.isValid = false;
        return;
      }
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.container .dadosCep {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.container .dadosError {
  color: red;
  margin-top: 10px;
}

h3 {
  margin-top: 40px;
}

input {
  margin-top: 20px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  margin-top: 10px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: #0b4b92;
  color: #fff;
  cursor: pointer;

  &:hover {
    background-color: #1d6bc4;
  }
}
</style>
