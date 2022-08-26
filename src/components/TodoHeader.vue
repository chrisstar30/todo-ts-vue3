<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'TodoHeader',
  props: {
    addTodo: {
      type: Function,
      required: true
    }
  },
  setup (props) {
    const tit = ref('')

    const addHandler = () => {
      if (!tit.value.trim()) {
        alert('不得為空')
        return false
      }
      const todo = {
        id: Date.now(),
        tit: tit.value,
        isCompleted: false
      }
      props.addTodo(todo)
      tit.value = ''
    }

    return {
      tit,
      addHandler
    }
  }
})
</script>

<template>
  <div class="todo-header">
    <input type="text" v-model="tit" @keyup.enter="addHandler">
    <div class="base-btn" @click="addHandler">送出</div>
  </div>
</template>
