<template>
  <div>
    <div class="adding-todo">
      <h1>Task List</h1>
      <span>
        <input v-model="newTodo" placeholder="Typing....." @keyup.enter="addTodo" />
        <button @click="addTodo">Add Task</button>
      </span>
    </div>

    <div class="todo-holder">
      <div v-for="todo in todos" :key="todo.id" class="each-todo">
        <span>
          <button v-if="!isEditing(todo.id)" @click="editTodo(todo)">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
            <path fill="currentColor" d="M25 4.031c-.766 0-1.516.297-2.094.875L13 14.781l-.219.219l-.062.313l-.688 3.5l-.312 1.468l1.469-.312l3.5-.688l.312-.062l.219-.219l9.875-9.906A2.968 2.968 0 0 0 25 4.03zm0 1.938c.234 0 .465.12.688.343c.445.446.445.93 0 1.375L16 17.376l-1.719.344l.344-1.719l9.688-9.688c.222-.222.453-.343.687-.343zM4 8v20h20V14.812l-2 2V26H6V10h9.188l2-2z"/>
            </svg>
          </button>
          <button v-if="!isEditing(todo.id)" @click="deleteTodo(todo.id)" class="delete">
           <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
           <path fill="currentColor" d="M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6v12M8 9h8v10H8V9m7.5-5l-1-1h-5l-1 1H5v2h14V4h-3.5Z"/>
           </svg>
          </button>
        </span>
        <p v-if="!isEditing(todo.id)">{{ todo.title }}</p>
        <input v-if="isEditing(todo.id)" v-model="editedTodoTitle" @blur="updateTodo(todo)"
          @keyup.enter="updateTodo(todo)" />
      </div>
    </div>
  </div>
</template>

<script>

export default {

  data() {
    return {
      newTodo: "",
      todos: [],
      editingTodoId: null,
      editedTodoTitle: "",
    };
  },

  mounted() {
    this.fetchTodos();
  },

  methods: {
    fetchTodos() {
      this.todos = [
        { id: 1, title: "Default card" },
      ];
    },

    addTodo() {
      if (this.newTodo.trim() !== "") {
        const newTodo = {
          id: Date.now(),
          title: this.newTodo,
        };
        this.todos.push(newTodo);
        this.newTodo = ""; // Clear the input field
      }
    },

    editTodo(todo) {
      this.editingTodoId = todo.id;
      this.editedTodoTitle = todo.title;
    },

    updateTodo(todo) {
      if (this.editedTodoTitle.trim() !== "") {
        todo.title = this.editedTodoTitle;
        this.editingTodoId = null;
        this.editedTodoTitle = "";
      }
    },

    deleteTodo(id) {
      let ask = confirm('Are you sure?')
      if (ask) {
        const index = this.todos.findIndex((todo) => todo.id === id);
        if (index !== -1) {
          this.todos.splice(index, 1);
        }
      }
    },

    isEditing(todoId) {
      return this.editingTodoId === todoId;
    },
  },
};
</script>