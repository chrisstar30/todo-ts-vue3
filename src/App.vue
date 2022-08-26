<script lang="ts">
import { defineComponent, reactive, provide, watch } from 'vue'
import TodoHeader from './components/TodoHeader.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoList from './components/TodoList.vue'
import type { ITodo } from './types/todo'

export default defineComponent({
  name: 'App',
  components: {
    TodoHeader,
    TodoFooter,
    TodoList
  },
  setup () {
    const getTodos = ():ITodo [] => {
      return JSON.parse(localStorage.getItem('todos') || '[]')
    }

    const setTodos = (todos: ITodo[]) => {
      localStorage.setItem('todos', JSON.stringify(todos))
    }

    const state = reactive<{todos: ITodo []}>({
      todos: []
    })

    state.todos = getTodos()

    const addTodo = (todo: ITodo) => {
      state.todos.unshift(todo)
    }

    const deleteTodo = (todoIndex: number) => {
      state.todos.splice(todoIndex, 1)
    }

    const completedTodo = (todoIndex: number, type: boolean) => {
      state.todos[todoIndex].isCompleted = type
    }

    const controlTodos = (type: boolean) => {
      state.todos.forEach((todo) => {
        todo.isCompleted = type
      })
    }

    const clearCompletedAll = () => {
      state.todos = state.todos.filter((todo) => !todo.isCompleted)
    }

    watch(
      () => state.todos,
      setTodos,
      { deep: true }
    )

    provide('deleteTodo', deleteTodo)
    provide('completedTodo', completedTodo)
    return {
      state,
      addTodo,
      controlTodos,
      clearCompletedAll
    }
  }
})
</script>

<template>
  <main>
    <h1>Vue.js + Typescript - TodoList</h1>
    <div class="todo-outer">
      <TodoHeader :addTodo="addTodo"/>
      <TodoList :todos="state.todos"/>
      <TodoFooter :todos="state.todos" :controlTodos="controlTodos" :clearCompletedAll="clearCompletedAll"/>
    </div>
  </main>
</template>
