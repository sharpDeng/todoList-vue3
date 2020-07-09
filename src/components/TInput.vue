<template>
  <input class="new-todo" v-model="currentValue" :placeholder="placeholder" @keyup.enter="enterHandler"/>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
  props:{
    placeholder: String
  },
  setup(prop, { emit }){
    let currentValue = ref('')

    function enterHandler() {
      if(!currentValue.value) return 
      emit('value', currentValue)
      emit('enterAdd', currentValue.value)
      currentValue.value = ''
    }
    return { 
      currentValue,
      enterHandler
    }
  },
  
}
</script>

<style scoped>
</style>
