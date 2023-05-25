<template>
  <div>
    <h2>Todo application</h2>
    <router-link to="/">Home</router-link>
    <hr>
    <AddTodo
        @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not-completed</option>
    </select>
    <hr>
    <Loader v-if="loading"/>
    <Todolist
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos"
    >
    </Todolist>
    <p v-else>No todos!</p>
  </div>
</template>

<script>
import {eventEmmiter} from "@/main"
import Todolist from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'

const toJSON = responce => responce.json()
const handleError = error => console.log(error)

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {

    const onFetchSuccess = json => {
      setTimeout(() => {
        this.todos = json
        this.loading = false
      }, 1000)
    }

    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
        .then(toJSON)
        .then(onFetchSuccess)
        .catch(handleError)

    eventEmmiter.$on("removeTodo", id => {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        method: 'DELETE',
      })
          .then(toJSON)
          .then(() => this.todos = this.todos.filter(t => t.id !== id))
          .catch(handleError)
    })

    eventEmmiter.$on("changeTodo", todo => {
      fetch('https://jsonplaceholder.typicode.com/todos/1', {
        method: 'PUT',
        body: JSON.stringify({
          title: todo.title,
          id: todo.id,
          completed: !todo.completed,
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      })
          .then(toJSON)
          .then((todoItem) => todo.completed = todoItem.completed)
          .catch(handleError)
    })

  },
  components: {
    Todolist,
    AddTodo,
    Loader
  },
  methods: {
    addTodo(todo) {
      fetch('https://jsonplaceholder.typicode.com/todos', {
        method: 'POST',
        body: JSON.stringify({
          title: todo.title,
          userId: 1,
          completed: false
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      })
          .then(toJSON)
          .then((todo) => {
            const newTodo = {...todo, id:Date.now()}
            this.todos.push(newTodo)
          })
          .catch(handleError)
    },
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos
      }
      if (this.filter === 'completed') {
        return this.todos.filter(t => t.completed)
      }
      if (this.filter === 'not-completed') {
        return this.todos.filter(t => !t.completed)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
