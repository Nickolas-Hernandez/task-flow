<script setup>
import { PlusCircleIcon, XCircleIcon, TrashIcon } from '@heroicons/vue/24/solid'
</script>

<template>
  <header class="max-w-2xl mx-auto">
    <h1 class="text-2xl text-center mb-4">TaskFlow</h1>
  </header>
  <main class="container max-w-2xl mx-auto">
    <form @submit.prevent="addTask" class="task-form task-form flex flex-row">
      <!-- <label for="new-task">Task:</label> -->
      <input
        v-model="newTask"
        name="new-task"
        type="text"
        class="border border-black rounded-lg items-center w-full pl-2"
        placeholder="Add a new task..."
      />
      <div class="task-form__action-buttons flex mx-auto">
        <button type="submit" class="my-auto">
          <PlusCircleIcon class="w-8 text-blue-500 hover:text-blue-600 duration-200" />
        </button>
      </div>
    </form>

    <div class="filters flex justify-center gap-4 mt-6">
      <button
        :class="['filters__button', activeFilter === 'all' ? 'active-filter' : '']"
        @click="setFilter('all')"
      >
        All
      </button>
      <button
        :class="['filters__button', activeFilter === 'active' ? 'active-filter' : '']"
        @click="setFilter('active')"
      >
        Active
      </button>
      <button
        :class="['filters__button', activeFilter === 'completed' ? 'active-filter' : '']"
        @click="setFilter('completed')"
      >
        Completed
      </button>
    </div>

    <div class="tasks-list">
      <h2 v-if="tasks.length === 0" class="text-gray-500 text-center mt-8">
        You have no tasks yet. Let&apos;s get productive!
      </h2>
      <ul v-else class="tasks-list__list">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          class="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition mb-4 flex justify-between items-center"
        >
          <input
            type="checkbox"
            :checked="task.completed"
            @change="updateTaskStatus(task.id)"
            class="mr-2"
          />
          <span
            :class="[
              'mr-auto',
              task.completed
                ? 'line-through text-gray-400 opacity-70 transition-colors duration-200'
                : 'text-gray-800 transition-colors duration-200',
            ]"
          >
            <p>{{ task.text }}</p>
          </span>
          <TrashIcon
            @click="deleteTask(task.id)"
            class="w-8 text-gray-500 hover:text-gray-700 transition duration-200"
          />
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
      activeFilter: 'All',
    }
  },
  computed: {
    filteredTasks() {
      if (this.activeFilter === 'active') {
        return this.tasks.filter((task) => !task.completed)
      } else if (this.activeFilter === 'completed') {
        return this.tasks.filter((task) => task.completed)
      } else {
        return this.tasks
      }
    },
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
      this.saveTasksToStorage()
    },
    updateTaskStatus(id) {
      const selectedTask = this.tasks.find((task) => task.id === id)
      if (selectedTask) {
        selectedTask.completed = !selectedTask.completed
        this.saveTasksToStorage()
      }
    },
    deleteTask(id) {
      const selectedTaskIndex = this.tasks.findIndex((task) => task.id === id)
      if (selectedTaskIndex !== -1) {
        this.tasks.splice(selectedTaskIndex, 1)
        this.saveTasksToStorage()
      }
    },
    setFilter(option) {
      console.log('option: ', option)
      if (this.activeFilter === option) return
      this.activeFilter = option
    },
    saveTasksToStorage() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
  },
  mounted() {
    const savedTasks = localStorage.getItem('tasks')

    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks)
    }
  },
}
</script>

<style>
.filters__button {
  @apply px-4 py-2 border border-gray-300 rounded-md text-gray-600 hover:text-blue-500 hover:bg-gray-100 transition-colors duration-200 cursor-pointer font-medium;
}
.active-filter {
  @apply text-blue-600 bg-blue-100 border-blue-400 font-semibold;
}
</style>
