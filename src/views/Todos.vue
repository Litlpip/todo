<template>
  <div>
    <h2>todo app</h2>
    <router-link to="/">Home</router-link>
    <hr>
    <AddTodo @add-todo='addTodo'/>
    <select v-model="filter">
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="not-completed">Not complited</option>
    </select>
    <hr>
    <Loader v-if="loading"/>
    <TodoList 
        v-else-if="filteredTodos.length" 
        v-bind:todos="filteredTodos" 
        @remove--todo="removeTodO"/>
    <p v-else>No todos!</p>
  </div>
</template>

<script>

import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'
export default {
  name: 'app',
  data(){
    return{
      todos:[],
      loading: true,
      filter: 'all'
    }

  },
//   watch:{
//       filter(value){
//           console.log(value)
//       }
//   },
  computed:{
      filteredTodos(){
          if(this.filter === 'all'){
              return this.todos
          }
          if(this.filter === 'completed'){
              return this.todos.filter(t => t.completed)
          }
          if(this.filter === 'not-completed'){
              return this.todos.filter(t => !t.completed)
          }
      }

  },
  components: {
    TodoList,
    AddTodo,
    Loader
  },
  mounted(){
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
      .then(response => response.json())
      .then(json => {
          setTimeout(()=>{
            this.todos = json
            this.loading = false
          },1000)
        
      })

  },
  methods:{
    removeTodO(id){
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo){
      this.todos.push(todo)
    }
  }
}
</script>