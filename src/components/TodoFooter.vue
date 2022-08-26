<script lang="ts">
import { defineComponent, computed } from 'vue'
import type { ITodo } from '../types/todo'

export default defineComponent({
  name: 'TodoFooter',
  props: {
    todos: {
      type: Array as () => ITodo[],
      required: true
    },
    controlTodos: {
      type: Function,
      required: true
    },
    clearCompletedAll: {
      type: Function,
      required: true
    }
  },
  setup (props) {
    const completedLength = computed(() => {
      return props.todos.reduce(
        (previousValue, todo) => previousValue + (todo.isCompleted ? 1 : 0), 0
      )
    })

    const controlAllHandler = computed({
      get: () => {
        return completedLength.value > 0 && completedLength.value === props.todos?.length
      },
      set: (val) => {
        props.controlTodos(val)
      }
    })

    return {
      completedLength,
      controlAllHandler
    }
  }
})
</script>

<template>
  <div class="todo-footer">
    <label for="all">
      <input type="checkbox" v-model="controlAllHandler" name="all" id="all">
      <div>全選</div>
    </label>
    <div class="info">
      <span>已完成 {{ completedLength }}</span>
      <span>/</span>
      <span>全部 {{ todos?.length }}</span>
    </div>
    <div class="base-btn" @click="clearCompletedAll()">清除已完成項目</div>
  </div>
</template>
