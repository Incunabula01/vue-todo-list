<template>
  <div id="appContainer">
    <h1>Todo List</h1>
    <form @submit.prevent="handleNewTodo()">
        <input type="text" v-model="newTodo" @keyup.enter="handleNewTodo()"/>
        <button class="waves-effect waves-light btn" >Add Todo</button>
    </form>
    <div id="todoList">
      <div 
        :class="{completed: todo.complete}" 
        v-for="(todo, index) in todos" 
        :key="index"
        @click="handleCompletedTodo(todo)"
        >

        <div class="row">
          <div class="col s12">
            <div class="card teal lighten-1">
              <div class="card-content white-text">
                <span class="card-title">{{ todo.text }}</span>
              </div>
            </div>
          </div>
        </div>
         
      </div>
      <button class="btn waves-effect waves-light red lighten-2" @click="handleRemoveTodos()" v-if="todos.length !== 0">
        Remove All Todos
      </button>
    </div>
  </div>

</template>

<script>
  import { ref } from 'vue';
  
  export default {
    name: 'App',
    setup() {
      const newTodo = ref("");
      const initLoadData = [{
        complete: false,
        text: 'Created Todo'
      }];

      let storedTodos;
      localStorage.getItem("todos") ? (storedTodos = JSON.parse(localStorage.getItem("todos"))) : (storedTodos = initLoadData);

      console.info('storedTodos', storedTodos);
      const todos = ref(storedTodos);

      function handleNewTodo(){
        if(newTodo.value !== ""){
          todos.value.push({
            complete: false,
            text: newTodo.value
          });
          updateStorage();
          newTodo.value = '';
        }
      }

      function handleCompletedTodo(todo){
        todo.complete = !todo.complete;
        updateStorage();
      }

      function handleRemoveTodos(){
        todos.value = [];
        updateStorage();
      }

      function updateStorage(){
        localStorage.setItem('todos', JSON.stringify(todos.value));
      }

      return {
        handleNewTodo,
        handleCompletedTodo,
        handleRemoveTodos,
        todos,
        newTodo
      }
    }
  }
</script>

<style scoped>
  #todoList {
    margin: 2%;
    padding: 0%;
  }

  #todoList div.completed {
    opacity: 0.5;
  }

  .card-content {
    cursor: pointer;
    padding: 0.5rem 1rem;
  }
  .card-content:hover {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.5);
  }

  #appContainer {
    padding: 2%;
  }

  

</style>
