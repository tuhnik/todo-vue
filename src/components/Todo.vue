<template>
  <div>
    <input type = "text" class = "input" placeholder = "What next?"
    v-model="newTodo" @keyup.enter="addTodo">
    <div class = "top-bar" v-if="todos.length">
        <button :class="{ active: filter === 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter === 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter === 'done' }"
        @click="filter = 'done'">Done</button>
    </div>
    <div class = "empty" v-if="!todosFiltered.length && todos.length">Nothing here...</div>
    <div v-for="(item, i) in todosFiltered" :key = "i" class = "item">
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
      <div><input class = "checkbox" type ="checkbox" :checked="noneRemaining"
      @change="checkAll($event)">Check all</div>
      <button v-if="showClearCompleted" class = "clear-completed"
      @click="clearCompleted()">Clear done</button>
      <div>{{remaining}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data() {
    return {
      filter: 'all',
      newTodo: '',
      editCache: '',
      todos: [{
        title: 'Mine poodi', completed: false, editing: false,
      },
      {
        title: 'Osta kartuleid', completed: false, editing: false,
      },
      ],
    };
  },
  computed: {
    remaining() {
      const remains = this.todos.filter(item => !item.completed).length;
      if (remains === 1) return 'Last item remaining!';
      return (remains) ? `${remains} items remaining` : 'All done!';
    },
    noneRemaining() {
      return this.todos.filter(item => !item.completed).length === 0;
    },
    showClearCompleted() {
      return this.todos.filter(item => item.completed).length;
    },
    todosFiltered() {
      if (this.filter === 'all') {
        return this.todos;
      } else if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
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

        title: this.newTodo,
        completed: false,
        editing: false,
      });
      this.newTodo = '';
    },
    removeTodo(i) {
      this.todos.splice(i, 1);
    },
    editTodo(item) {
      this.editCache = item.title; // eslint-disable-line no-param-reassign
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
  mounted() {
    if (localStorage.todos && localStorage.todos.length > 2) {
      this.todos = JSON.parse(localStorage.todos);
    }
  },
  watch: {
    todos: {
      handler() {
        localStorage.todos = JSON.stringify(this.todos);
      },
      deep: true,

    },
  },
};
</script>


<style lang = "scss">

.input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  outline: none;
  border: 1px solid #bdc3c7;
  border-radius: 5px;

  &:focus{
    border: 1px solid rgb(101, 184, 199);
    }
}

.item {
  font-size: 16px;
  width: 100%;
  margin-bottom: 12px;
  display: flex;
  justify-content: space-between;
  background: rgb(236, 236, 236);
  padding: 10px 18px;
  border-radius: 5px;
  word-break: break-all;

}
.item-left {
  width: 100%;
  display: flex;
}
.item-label {
  align-self: center;
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
  margin-left: 10px;
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

.clear-completed {
  z-index: 100;
  display: block;
}
.checkbox {
  margin-right: 10px;
  align-self: top;
}

.empty {
  align-self: top;
  font-size: 16px;
  width: 100%;
  margin-bottom: 12px;
  color: gray;

}

.bottom-bar{
  margin-bottom: 12px;
  border-top: 1px solid rgb(223, 221, 221);
  padding: 10px 18px;
  color: gray;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.top-bar {
  margin-bottom: 12px;
  border-bottom: 1px solid rgb(223, 221, 221);
  padding: 10px 18px;
  color: gray;
  display: flex;
  justify-content: center;
}

.top-bar > button,.bottom-bar > button{
  width: 120px;
  margin: 5px;
  border: none;
  border-radius: 5px;
  color: white;
  text-align: center;
  outline: none;
  padding: 5px;
  display: inline-block;
  font-size: 16px;
  &:hover{
    background-color: gray;
    }
}


.active {
  background-color: rgb(101, 184, 199);
}

</style>
