<template>
  <div class="container">
    <img alt="ToDo logo" src="./assets/logo.png" height="200px">
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
  display: flex;
  flex-direction: column;
  width: 600px;
  margin: auto;
  align-items: center;
  font-family: Helvetica;
}
.container > div {
  width: 100%
}
</style>
