<template>
  <img alt="Vue logo" src="./assets/tradeupgroup_logo.jpeg" />
  <app-form-cpf msg="Bem vindo - API cep" />
</template>

<script>
import FormCpf from "./components/FormCpf.vue";

export default {
  name: "App",
  components: {
    "app-form-cpf": FormCpf,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
