<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-on:del-todo="deleteTodo" v-bind:todos="todos"/>
  </div>
</template>

<script>
import axios from "axios";
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    async deleteTodo(id) {
      try {
        let res = await axios.delete(
          `https://jsonplaceholder.typicode.com/todos/${id}`
        );
        if (res.data) {
          this.todos = this.todos.filter(todo => todo.id !== id);
        }
      } catch (error) {}
    },
    async addTodo(todo) {
      const { title, completed } = todo;
      try {
        let res = await axios.post(
          "https://jsonplaceholder.typicode.com/todos",
          {
            title,
            completed
          }
        );
        if (res.data) {
          this.todos = [...this.todos, res.data];
        }
        console.log(res);
      } catch (error) {
        console.log(error);
      }
    }
  },
  async created() {
    try {
      let res = await axios.get(
        "https://jsonplaceholder.typicode.com/todos?_limit=5"
      );
      if (res.data) {
        this.todos = res.data;
      }
    } catch (error) {
      if (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style>
* {
  box-shadow: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
</style>
