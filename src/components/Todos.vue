<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input
        type="text"
        class="new-todo"
        placeholder="Ajouter une tâche"
        v-model="newTodo"
        @keyup.enter="addTodo(newTodo)"
      />
    </header>
    <div class="main">
      <input
        id="toggle-all"
        class="toggle-all"
        type="checkbox"
        v-model="allDone"
      />
      <label for="toggle-all"></label>
      <ul class="todo-list">
        <li
          class="todo"
          v-for="todo in filteredTodos"
          :key="todo.name"
          :class="{ completed: todo.completed, editing: todo === editing }"
        >
          <div class="view">
            <input type="checkbox" v-model="todo.completed" class="toggle" />
            <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input
            type="text"
            class="edit"
            v-model="todo.name"
            @keyup.enter="doneEdit"
            @keyup.esc="cancelEdit"
            @blur="doneEdit"
            v-focus="todo === editing"
          />
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count">
        <strong>{{ remainingTodosCount }}</strong>
        Tâches à faire
      </span>
      <ul class="filters">
        <li>
          <a
            href="#"
            :class="{ selected: filter === 'all' }"
            @click.prevent="filter = 'all'"
            >All</a
          >
        </li>
        <li>
          <a
            href="#"
            :class="{ selected: filter === 'todo' }"
            @click.prevent="filter = 'todo'"
            >To do</a
          >
        </li>
        <li>
          <a
            href="#"
            :class="{ selected: filter === 'done' }"
            @click.prevent="filter = 'done'"
            >Done</a
          >
        </li>
      </ul>
      <button
        class="clear-completed"
        v-show="completedTodosCount"
        @click.prevent="deleteCompleted"
      >
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
import Vue from "vue";
import Vuex from "vuex";
import store from "./TodosStore";
global.v = Vuex;

export default {
  store: store,
  props: {
    value: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {
      //todos: this.value,
      newTodo: "",
      filter: "all",
      editing: null,
      oldTodo: ""
    };
  },
  watch: {
    value(value) {
      this.todos = value;
    }
  },
  methods: {
    ...Vuex.mapActions({ addTodoStore: "addTodo", deleteTodo: "deleteTodo" }),
    addTodo() {
      this.addTodoStore(this.newTodo);
      this.newTodo = "";
    },
    // Set par vuex
    // addTodo() {
    //   this.todos.push({
    //     completed: false,
    //     name: this.newTodo
    //   });
    //   this.newTodo = "";
    // },
    // deleteTodo(todo) {
    //   this.todos = this.todos.filter(i => i !== todo);
    //   this.$emit("input", this.todos);
    // },
    deleteCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
      this.$emit("input", this.todos);
    },
    editTodo(todo) {
      this.editing = todo;
      this.oldTodo = todo.name;
    },
    doneEdit() {
      this.editing = null;
    },
    cancelEdit() {
      this.editing.name = this.oldTodo;
      this.doneEdit();
    }
  },
  computed: {
    ...Vuex.mapGetters([
      "todos",
      "remainingTodosCount",
      "completedTodosCount",
      "completedTodos",
      "remainingTodos"
    ]),
    allDone: {
      get() {
        return this.remaining === 0;
      },
      set(value) {
        this.todos.forEach(todo => {
          todo.completed = value;
        });
      }
    },
    // Set via VUEX
    // remaining() {
    //   return this.todos.filter(todo => !todo.completed).length;
    // },
    // completed() {
    //   return this.todos.filter(todo => todo.completed).length;
    // },
    hasTodos() {
      return this.todos.length > 0;
    },
    filteredTodos() {
      if (this.filter === "todo") {
        return this.remainingTodos;
      } else if (this.filter === "done") {
        return this.completedTodos;
      }
      return this.todos;
    }
  },
  directives: {
    focus(el, value) {
      if (value) {
        Vue.nextTick(_ => el.focus());
      }
    }
  }
};
</script>
<style src="./todos.css"></style>
