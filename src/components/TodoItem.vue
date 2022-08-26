<script lang="ts">
import { defineComponent, inject, computed } from 'vue'
import type { ITodo } from '../types/todo'

export default defineComponent({
  name: 'TodoItem',
  props: {
    todo: {
      type: Object as () => ITodo,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  setup (props) {
    const deleteTodo = inject('deleteTodo') as (todoIndex: number) => void
    const completedTodo = inject('completedTodo') as (todoIndex: number, type: boolean) => void

    const deleteHandler = () => {
      if (window.confirm('是否要刪除')) {
        deleteTodo(props.index)
      }
    }

    const completedHandler = computed({
      get: () => props.todo.isCompleted,
      set: (val) => completedTodo(props.index, val)
    })

    return {
      deleteHandler,
      completedHandler
    }
  }
})
</script>

<template>
  <li class="todo-item">
    <input type="checkbox" v-model="completedHandler" :name="'todo' + todo?.id" :id="'todo' + todo?.id">
    <div class="todo-inner">
      <label :for="'todo' + todo?.id" class="todo-tit">
        <div>{{ todo?.tit }}</div>
      </label>
      <div class="todo-tool">
        <i class="fa fa-refresh update-btn"></i>
        <i class="fa fa-trash delete-btn" @click="deleteHandler"></i>
      </div>
    </div>
  </li>
</template>
