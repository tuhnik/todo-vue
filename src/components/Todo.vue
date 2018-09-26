<template>
  <div>
    <input type = "text" class = "input" placeholder = "What next?"
    v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(item, i) in todos" :key = "item.id" class = "item">
      <div class = "item-left" @dblclick="editTodo(item)">
      <input type = "checkbox" class = "checkbox" v-model ="item.completed">
      <div v-if="!item.editing" class="item-label"
      :class="{completed: item.completed}">{{item.title}}</div>
      <input v-else class= "item-edit" v-model="item.title" @blur="doneEditing(item)"
      @keyup.enter="doneEditing(item)"
      @keyup.esc="cancelEditing(item)"
       type = "text"
      v-focus>
    </div>
      <div class = "close" @click="removeTodo(i)">&#10006;</div>
    </div>
    <div class = "bottom-bar" v-if="todos.length">
      <div><input type ="checkbox" :checked="noneRemaining"
      @change="checkAll($event)">Check All</div>
      <button v-if="showClearCompleted" class = "clear-completed"
      @click="clearCompleted()">Clear Completed</button>
      <div>{{remaining}} items left</div>
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
      editCache: '',
      todos: [{
        id: 1, title: 'Mine poodi', completed: false, editing: false,
      },
      {
        id: 2, title: 'Osta kartuleid', completed: false, editing: false,
      },
      ],
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(item => !item.completed).length;
    },
    noneRemaining() {
      return this.todos.filter(item => !item.completed).length === 0;
    },
    showClearCompleted() {
      return this.todos.filter(item => item.completed).length;
    },


  },
  directives: {
    focus: {
      inserted(el) {
        el.focus();
      },
    },
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
        editing: false,
      });
      this.newTodo = '';
      this.todoId += 1;
    },
    removeTodo(i) {
      this.todos.splice(i, 1);
    },
    editTodo(item) {
      this.editCache = item.title;
      item.editing = true; // eslint-disable-line no-param-reassign
    },
    doneEditing(item) {
      if (!item.title.trim()) {
        return;
      }
      item.editing = false; // eslint-disable-line no-param-reassign
    },
    cancelEditing(item) {
      item.title = this.editCache; // eslint-disable-line no-param-reassign
      item.editing = false; // eslint-disable-line no-param-reassign
    },
    checkAll(evt) {
      this.todos.forEach((item) => {
        item.completed = evt.target.checked; // eslint-disable-line no-param-reassign
      });
    },
    clearCompleted() {
      this.todos = this.todos.filter(el => !el.completed);
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
  width: 100%;
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgb(236, 236, 236);
  padding: 10px 18px;
  border-radius: 5px;
}
.item-left {
  width: 100%;
  display: flex;
  align-items: center;
}
.item-edit {
  width: 100%;
  font-size: 16px;
  background: rgb(236, 236, 236);
  outline: none;
  border: none;

}

.close {
  cursor: pointer;
  user-select: none;
  color: gray;
  &:hover{
    color: black;
    }
}

.completed {
  color: gray;
  text-decoration: line-through;
}

.checkbox {
  margin-right: 10px;
}
.bottom-bar{
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  color: gray;
  display: flex;
  justify-content: space-between;
}

</style>
