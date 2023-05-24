<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model="title">
    <button type="submit">Create</button>
  </form>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
  export default {
    data(){
      return{
        title: ''
      }
    },
    methods:{
      onSubmit(){
        if(this.title.trim()){
          const newTodo = {
            // id: Date.now(),
            id: uuidv4(),
            title: this.title,
            completed: false
          }
          this.$emit('add-todo',newTodo)
          this.title = ''
        }
      }
    }
  }
</script>

<style scoped>
  form{
    display: flex;
    justify-content: center;
  }
  input{
    width: 400px;
  }
  button{
    margin-left: 10px;
  }
</style>