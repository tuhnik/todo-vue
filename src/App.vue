<template>
  <div class="container">
    <img alt="ToDo logo" src="./assets/logo.png">
    <Todo/>
  </div>

</template>

<script>
import Todo from './components/Todo.vue';

export default {
  name: 'app',
  components: {
    Todo,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
.container {
  width: 50%;
  font-family: Helvetica;
}

</style>
