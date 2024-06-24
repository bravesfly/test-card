<template>
  <div id="app">
    <CreditCardForm />
  </div>
</template>

<script>

import CreditCardForm from './components/CreditCardForm.vue'

export default {
  name: 'App',
  components: {
    CreditCardForm
  }
}
</script>

<style>
@import '@/assets/style.css';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
