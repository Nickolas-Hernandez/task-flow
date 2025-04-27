<script setup>
import { PlusCircleIcon, XCircleIcon } from '@heroicons/vue/24/solid'
</script>

<template>
  <header>
    <h1 class="text-2xl text-center mb-4">TaskFlow</h1>
  </header>
  <main>
    <div id="todo-board__container" class="text-center">
      <form @submit.prevent="addTask" class="task-form flex flex-col">
        <label for="new-task">Task:</label>
        <input v-model="newTask" name="new-task" type="text" class="border border-black" />
        <div class="task-form__action-buttons flex mx-auto">
          <XCircleIcon
            @click="clearTask"
            class="w-8 text-gray-500 hover:text-gray-700 transition duration-200"
          />
          <button type="submit">
            <PlusCircleIcon class="w-8 text-blue-500 hover:text-blue-600 duration-200" />
          </button>
        </div>
      </form>
    </div>
    <div class="tasks__container">
      <ul class="tasks__list">
        <li v-for="task in tasks" :key="task.id">
          {{ task.text }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
    }
  },
  methods: {
    addTask() {
      if (!this.newTask) {
        return
      }
      const trimmedTask = this.newTask.trim()
      console.log('trimmedTask: ', trimmedTask)
      this.tasks.push({
        id: Date.now(),
        text: trimmedTask,
        completed: false,
      })
      this.newTask = ''
    },
    clearTask() {
      this.newTask = ''
    },
  },
}
</script>

<style></style>
