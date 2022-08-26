<script lang="ts">
import { defineComponent } from 'vue'
import TodoItem from './TodoItem.vue'

export default defineComponent({
  name: 'TodoList',
  components: {
    TodoItem
  },
  props: {
    todos: {
      type: Array,
      required: true
    }
  }
})
</script>

<template>
  <div>
    <div class="todo-list">
      <ul>
        <TodoItem v-for="(todo, index) in todos" :key="index + 'todo'" :todo="todo" :index="index"/>
      </ul>
    </div>
  </div>
</template>
