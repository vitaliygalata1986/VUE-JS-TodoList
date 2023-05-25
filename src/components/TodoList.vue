<template>
  <div>
    <ul>
      <TodoItem
       v-for="(todo, i) in todos"
       v-bind:todo="todo"
       v-bind:index="i"
      >
      </TodoItem>
    </ul>
  </div>
</template>

<script>
  import TodoItem from '@/components/TodoItem'
  export default {
    props:['todos'],
    components:{
      TodoItem
    },
  }
</script>

<style scoped>
ul{
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>