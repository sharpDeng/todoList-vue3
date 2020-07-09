<template>
  <li class="todo" :class="{completed: todo.completed, editing: isEdited}">
    <div class="view">
      <input class="toggle" type="checkbox" v-model="todo.completed">
      <label @dblclick="editTodo(todo)">{{todo.title}}</label>
      <button class="destroy" @click="removeTodo(todo)"></button>
    </div>
    <input class="edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" v-todo-focus="isEdited" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)">
  </li>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
  props: {
    todo: Object,
    currentIndex: Number
  },
  directives: {
    'todo-focus': function (el, binding) {
      if (binding.value) {
        el.focus();
      }
    }
  },
  setup(props, { emit }) {
    let todo = reactive(props.todo)
    let isEdited = ref(false)
    
    let beforeEditCache = ''
    function editTodo(todo) {
      isEdited.value = true
      beforeEditCache = todo.title
    }

    function removeTodo(todo) {
      emit('removeTodo', todo)
    }

    function doneEdit() {
      isEdited.value = false
    }

    function cancelEdit(todo) {
      isEdited.value = false
      todo.title = beforeEditCache
    }

    return {
      todo,
      editTodo,
      isEdited,
      removeTodo,
      doneEdit,
      cancelEdit
    }
  }
}
</script>