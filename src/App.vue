<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  components: {
    TodoList,
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-image: url('./components/backg.jpg');
  background-size: cover;
  background-position: center;
  height: 100vh; /* Ensure the image covers the entire viewport height */
}
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}
</style>
