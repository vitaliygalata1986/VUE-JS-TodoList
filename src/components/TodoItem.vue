<template>
  <li>
        <span v-bind:class="{done: todo.completed}">
          <!--<input type="checkbox" v-on:change="todo.completed = !todo.completed">-->
          <input type="checkbox" @change="changeTodo(todo)">
          <strong>{{ index + 1 }}</strong>
          {{ todo.title | uppercase }}
        </span>
        <button @click="removeTodo(todo.id)" class="delete">&times;</button>
  </li>
</template>

<script>
import {eventEmmiter} from "@/main"
export default {
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: Number
  },
  filters: {
    uppercase(value) {
      return value.toUpperCase()
    }
  },
  methods:{
    removeTodo(id){
      eventEmmiter.$emit("removeTodo",id)
    },
    changeTodo(todo){
      eventEmmiter.$emit("changeTodo",todo)
    }
  }
}
</script>

<style scoped>
li {
  border: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  padding: .5rem .2rem;
  margin-bottom: 1em;
}

.delete {
  background: red;
  color: #fff;
  border-radius: 50%;
  font-weight: bold;
  outline: none;
}

.done {
  text-decoration: line-through;
}

input {
  margin-right: 1rem;
}

button {
  cursor: pointer;
}
</style>