<template>
  <div>
    <input type = "text" class = "input" placeholder = "What next?"
    v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(item, i) in todos" :key = "item.id" class = "item">
      <div>{{item.id}}. {{item.title}}</div>
      <div class = "close" @click="removeTodo(i)">&#10006;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data() {
    return {
      todoId: 3,
      newTodo: '',
      todos: [{ id: 1, title: 'Mine poodi', completed: false },
        { id: 2, title: 'Osta kartuleid', completed: false },
      ],
    };
  },
  methods: {
    addTodo() {
      if (!this.newTodo.trim()) {
        return;
      }
      this.todos.push({
        id: this.todoId,
        title: this.newTodo,
        completed: false,
      });
      this.newTodo = '';
      this.todoId += 1;
    },
    removeTodo(i) {
      this.todos.splice(i, 1);
    },
  },
};
</script>


<style lang = "scss">

.input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  outline: none;
  border: 1px solid #bdc3c7;
  border-radius: 5px;

  &:focus{
    border: 1px solid #3498db;
    }
}

.item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgb(236, 236, 236);
  padding: 10px 18px;
  border-radius: 5px;
}

.close {
  cursor: pointer;
  user-select: none;
  color: gray;
  &:hover{
    color: black;
    }
}


</style>
