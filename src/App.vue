<template>
<div class="body">
  <HeaderVue />
  <div class="main">
    <div class="center-card">
      <div class="top-part">
        <input type="text" name="todo" id="todo" v-model="todoText" />
        <button @click="addTodoToList">
          <span v-if="currentEditIndex == -1">Add Todo</span>
          <span v-else>Edit Todo</span>
        </button>
      </div>

      <div class="todo-container">
        <div v-for="(todo, index) in todos" class="todo-item" :key="index">
          
          <div class="left">
            <input type="checkbox" v-model="todo.isDone">
            <p :class="todo.isDone ? `done`: ``">{{todo.text}}</p>
          </div>

          <div class="right">
          <button @click="editTodo(index)">Edit</button>
            <button class="remove-todo" @click="removeTodo(index)">X</button>
          </div>

        </div>


      </div>
    </div>
  </div>
  <FooterVue />
</div>
</template>

<script>

import HeaderVue from "./components/Header.vue"
import FooterVue from "./components/Footer.vue"
import { ref } from '@vue/reactivity';

export default {
  name: 'App',
  components: {
    HeaderVue,
    FooterVue
  },
  setup(){
    let currentEditIndex = ref(-1);
    let todoText = ref();
    let todos = ref([]);

    let editTodo = (index) => {
      currentEditIndex.value = index;
      todoText.value = todos.value[index].text;
    }

    let addTodoToList = () => {
      
      if(currentEditIndex.value == -1){
        todos.value = [...todos.value, {text: todoText.value, isDone: false}];
        
      }else{
        let todoInQ = todos.value[currentEditIndex.value]

        todos.value[currentEditIndex.value] = {
          text: todoText.value,
          isDone: todoInQ.isDone
        }
      }
      currentEditIndex.value = -1
      todoText.value = ""
    }

    let removeTodo = (index) => {
      todos.value = todos.value.filter((e,i) => i !== index)
    }
    return {
      todoText,
      todos,
      currentEditIndex,
      editTodo,
      addTodoToList,
      removeTodo
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

.body{
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main{
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}


.top-part{
  padding: 20px;
  background: #3d3d3d;
}

button, input{
  padding: 10px 20px;
  font-size: 1.2rem;
  outline: none;
  border: none;
}

button{
  background-color: #a0bfff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover{
  background-color: #588ffd;
}

.todo-item{
  background: #3d3d3d;
  color: white;
  padding: 10px 20px;
  margin: 5px 0;
  font-size: 1.2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.remove-todo{
  background-color: #ff6874;
}

.remove-todo:hover{
  background-color: #ff4050;
}

.done{
  color: greenyellow;
  text-decoration: line-through;
}

.left{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
}


</style>
