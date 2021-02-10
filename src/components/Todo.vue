<template>
  <div>
    <section class="todoapp">
      <header class="header">
        <h1>{{ title }}</h1>
        <input
            class="new-todo"
            placeholder="What needs to be done?"
            v-on:keyup.enter="createTodo"
            autofocus
        />
      </header>

      <!-- This section should be hidden by default and shown when there are todos -->
      <section class="main" v-if="todos.length">
        <input id="toggle-all" class="toggle-all" type="checkbox" />
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
          <!-- These are here just to show the structure of the list items -->
          <!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
          <li
              v-for="todo in todos"
              :key="todo.text"
              :class="{ completed: todo.isDone, editing: todo === editing }"
          >
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.isDone" />
              <label @dblclick="startEditing(todo)">{{ todo.text }}</label>
              <button class="destroy" @click="destroyTodo(todo)"></button>
            </div>
            <input
                class="edit"
                @keyup.esc="cancelEditing"
                @keyup.enter="finishEditing"
                @blur="finishEditing"
                :value="todo.text"
            />
          </li>
        </ul>
      </section>

      <footer class="footer" v-if="todos.length">
      <span class="todo-count">
          <strong>{{ activeTodos.length }}</strong> item(s) left
      </span>
        <button class="clear-completed" @click="clearCompleted" v-show="completedTodos.length">
          Clear completed
        </button>
      </footer>
    </section>
  </div>
</template>

<script>

const LOCAL_STORAGE_KEY = 'todo-app-vue';

export default {
  name: "todo",
  data() {
    return {
      title: 'Todo-List',
      todos: JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY)) || [
        { text: "Learn JavaScript ES6+ goodies", isDone: false },
        { text: "Learn Vue", isDone: false },
      ],
      editing: null
    };
  },
  methods: {
    createTodo(event) {
      const textbox = event.target;
      if (textbox.value != ' ' && textbox.value != '') {
        this.todos.push({text: textbox.value, isDone: false});
      }
      textbox.value = '';
    },
    startEditing(todo) {
      this.editing = todo;
    },
    finishEditing(event) {
      if (!this.editing) {
        return;
      }
      const textbox = event.target;
      this.editing.text = textbox.value.trim();
      this.editing = null;
    },
    cancelEditing() {
      this.editing = null;
    },
    destroyTodo(todo) {
      const index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    clearCompleted() {
      this.todos = this.activeTodos;
    },
  },
  computed: {
    activeTodos() {
      return this.todos.filter(t => !t.isDone);
    },
    completedTodos() {
      return this.todos.filter(t => t.isDone);
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(newValue) {
        localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(newValue));
      }
    }
  },
};
</script>