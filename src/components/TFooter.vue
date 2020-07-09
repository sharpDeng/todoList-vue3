<template>
  <footer class="footer">
    <span class="todo-count">
      <strong v-text="remaining"></strong> {{leftLabel}} left
    </span>
    <ul class="filters" @click="toggleActive">
      <li><a id="all" :class="{selected: currentValue == 'all'}">All</a></li>
      <li><a id="active" :class="{selected: currentValue == 'active'}">Active</a></li>
      <li><a id="completed" :class="{selected: currentValue == 'completed'}">Completed</a></li>
    </ul>
    <button class="clear-completed" @click="removeCompleted">
      Clear completed
    </button>
  </footer>
</template>

<script>
import { computed, ref } from "vue"

export default {
  props: {
    value: String,
    remaining: Number
  },
  setup(props, { emit }) {
    let currentValue = ref(props.value)

    let leftLabel = computed(() => {
      return 'item' + (props.remaining === 1 ? '' : 's') 
    })
    
    function removeCompleted(){
      emit('removeAll')
    }

    function toggleActive(e) {
      let val = e.target.id 
      if(val){
        currentValue.value = val
        emit('change', val)
      }
    }

    return {
      leftLabel,
      currentValue,
      removeCompleted,
      toggleActive
    }
  }
}
</script>