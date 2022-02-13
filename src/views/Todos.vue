<template>
  <div>
    <h2>Todos</h2>

    <form v-on:submit.prevent="addTodo">
      <div>
        <input type="text" v-model="body">
      </div>

      <div>
        <button type="submit" v-bind:disabled="!body.length">Добавить</button>
      </div>
    </form>

    <div>
      {{ filter }}
      <select v-model="filter" v-on:input="saveTodos">
        <option v-bind:value="1">Все</option>
        <option v-bind:value="2">Выполненные</option>
        <option v-bind:value="3">Невыполненные</option>
      </select>
    </div>

    <div>
      <ul>
        <li v-for="(todo, idx) of filteredTodos" v-bind:key="todo.id">
          <b>{{ idx + 1 }}</b>
          <span>.&nbsp;</span>
          <input type="checkbox" v-model="todo.completed">
          <span>&nbsp;</span>
          <span style="text-decoration: underline;">{{ todo.body }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      body: '',
      filter: 3,
      todos: []
    }),

    methods: {
      addTodo() {
        const body = this.body.trim()
        const todo = { id: Date.now(), body, completed: false }
        this.todos.push( todo )

        this.body = ''

        this.saveTodos()
      },

      saveTodos() {
        localStorage.setItem( 'todos', JSON.stringify( this.todos ) )
      },

      getTodos() {
        this.todos = JSON.parse( localStorage.getItem('todos') ) || []
      }
    },

    computed: {
      filteredTodos() {
        let todos

        if ( this.filter == 2 ) todos = this.todos.filter( todo => todo.completed )
        else if ( this.filter == 3 ) todos = this.todos.filter( todo => !todo.completed )
        else todos = this.todos
      
        return todos
      }
    },

    mounted() {
      this.getTodos()

      console.log(this.todos)
    }
  }
</script>
