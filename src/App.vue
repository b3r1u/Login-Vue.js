<template>
  <div>
    <AuthForm/>
  </div>
</template>

<script>
import AuthForm from './components/AuthForm.vue';

export default {
  name: 'App',
  components: {
    AuthForm
  }
}
</script>

<style>

body{
  background-color: #2f2b83;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
