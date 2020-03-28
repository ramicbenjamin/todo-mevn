<template lang="html">
  <div class="flex flex-col h-screen w-full bg-gray-200">
    <div class="fixed flex items-center justify-center bg-blue-500 h-16 w-full">
      <input
          class="flex items-center py-2 px-4 w-1/2 rounded-lg placeholder-gray-400 focus:outline-none"
          type="text"
          placeholder="Enter your to do!"
          v-model='newTodo'
        >

        <button class="py-2 px-4 text-white bg-green-500 hover:text-gray-200 hover:bg-green-600 rounded-lg ml-4 focus:outline-none" v-on:click='addTodo($event)'>
           Add!
        </button>
    </div>
 
    <div class="flex flex-col items-center bg-blue-300 h-screen mt-16 py-10">
      <div class="flex w-1/2 my-2" v-for='todo in todos' :key='todo._id'>
        <p class="flex-1 py-2 px-4 rounded-lg bg-gray-300 border">{{todo.title}}</p>
        
        <div class="flex items-center justify-center">
          <button class="py-2 px-4 text-white bg-red-500 hover:text-gray-200 hover:bg-red-600 rounded-lg ml-4 focus:outline-none" @click='deleteTodo(todo._id)'>
            Done
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import ToDoAPI from '@/services/ToDoAPI.js'
export default {
  data () {
    return {
      newTodo: '',
      todos: []
    }
  },
  mounted () {
    this.loadTodos()
  },

  methods: {
    async addTodo (evt) {
      evt.preventDefault() // prevents the form's default action from redirecting the page
      const response = await ToDoAPI.addTodo(this.newTodo)
      this.todos.push(response.data)
      this.newTodo = '' // clear the input field
    },

    async loadTodos () {
      const response = await ToDoAPI.getToDos()
      this.todos = response.data;
    },

    deleteTodo (todoID) {
      ToDoAPI.deleteTodo(todoID)
      // remove the array element with the matching id
      this.todos = this.todos.filter(function (obj) {
        return obj._id !== todoID
      })
    }
  }
}
</script>
