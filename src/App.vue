<template>
  <QuizComponent msg="Welcome to Your Vue.js App" />
</template>

<script>
import QuizComponent from './components/QuizComponent.vue';

export default {
  name: 'App',
  components: {
    QuizComponent,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #eee;
  height: 100vh;
  overflow-y: scroll;

}
</style>
