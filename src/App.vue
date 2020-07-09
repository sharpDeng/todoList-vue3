<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <section class="todoapp">
    <header class="header">
      <t-input v-model="newTodo" placeholder="What needs to be done?" @enterAdd="addTodo"></t-input>
    </header>
    <section class="main" v-show="todoList.length">
      <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone">
			<label for="toggle-all">Mark all as complete</label>
      <t-list v-show="todoList.length > 0">
        <t-item v-for="(todo,index) in filteredTodos" :key="todo.id" :todo="todo"  @removeTodo="removeTodo(index)"></t-item>
      </t-list>
    </section>
    <t-footer v-model="visibility" v-show="todoList.length" :remaining="filteredTodos.length" @change="changeHandle" @removeAll="removeAll(todoList)"> </t-footer>
  </section>

</template>

<script>
import TInput from './components/TInput.vue'
import TList from './components/TList.vue'
import TItem from './components/TItem.vue'
import TFooter from './components/TFooter.vue'
import { reactive, ref, computed, watch, toRefs } from 'vue' 

let uid = 0

var filters = {
  all: function (todos) {
    return todos;
  },
  active: function (todos) {
    return todos.filter(function (todo) {
      return !todo.completed;
    });
  },
  completed: function (todos) {
    return todos.filter(function (todo) {
      return todo.completed;
    });
  }
};

export default {
  name: 'App',
  components: {
    TInput,
    TList,
    TItem,
    TFooter
  },
  setup() {
    let todoList = reactive([{completed: false, title: 'sdf', id: 324}])

    let todoData = reactive({
      newTodo: '',
      allDone: false, 
      visibility: 'all'
    })

    watch(() => todoData.allDone, (val)=> {
        todoList.map(item => {
          item.completed = val
        })
    })

    let filteredTodos = computed(() => {
      return filters[todoData.visibility](todoList)
    })

    function addTodo(newItem) {
      todoList.push({title: newItem, completed: false, id: uid++})
    }

    function removeTodo(index) {
      todoList.splice(index, 1)
    }

    function changeHandle(vi) {
      todoData.visibility = vi
    }



    return { 
      ...toRefs(todoData),
      todoList,
      filteredTodos,
      addTodo,
      removeTodo,
      changeHandle,
      removeAll
    }
    
  }
}

function removeAll(todoList){
  console.log(todoList)
  todoList.splice(0)
}
</script>


