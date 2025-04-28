<script setup>
import { PlusCircleIcon, XCircleIcon, TrashIcon } from '@heroicons/vue/24/solid'
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
      <ul v-if="tasks[0]" class="tasks__list">
        <li
          v-for="task in tasks"
          :key="task.id"
          class="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition mb-4 flex justify-between items-center"
        >
          <input
            type="checkbox"
            :checked="task.completed"
            @change="updateTaskStatus(task.id)"
            class="mr-2"
          />
          <p class="mr-auto inline">{{ task.text }}</p>
          <TrashIcon
            @click="deleteTask(task.id)"
            class="w-8 text-gray-500 hover:text-gray-700 transition duration-200"
          />
        </li>
      </ul>
      <h2 v-else class="text-gray-500 text-center mt-8">
        “You have no tasks yet. Let&apos;s get productive!”
      </h2>
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
    updateTaskStatus(id) {
      const selectedTask = this.tasks.find((task) => task.id === id)
      if (selectedTask) {
        selectedTask.completed = !selectedTask.completed
      }
    },
    deleteTask(id) {
      const selectedTaskIndex = this.tasks.findIndex((task) => task.id === id)
      console.log('Selected Task Index: ', selectedTaskIndex)
      if (selectedTaskIndex !== -1) {
        this.tasks.splice(selectedTaskIndex, 1)
      }
      console.log('this.tasks: ', this.tasks)

    },
  },
}
</script>

<style></style>
