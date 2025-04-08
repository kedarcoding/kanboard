<template>
    <div class="py-4 px-6">
        <div class="font-semibold text-blue-900 text-xl py-3">
              Task Management
       </div>
      <!-- Draggable Columns -->
      <draggable v-model="columnOrder" item-key="status" class="flex gap-4" group="columns" direction="horizontal">
        <template #item="{ element: column }">
          <div class="w-1/3 border p-2 shadow-md">
            <h2 class="text-lg font-bold capitalize mb-2">{{ column.status }}</h2>
  
            <!-- Draggable Tasks -->
            <draggable
          v-model="column.tasks"
          group="tasks"
          item-key="id"
          @change="onTaskChange($event, column.status)"
        >
              <template #item="{ element: task }">
                <div class="bg-gray-200 hover:bg-gray-300 p-3 my-2 rounded cursor-pointer">
                  <div v-if="!task.editing">
                    <div class="text-lg text-gray-800 font-semibold">{{ task.title }}</div>
                    <div class="flex justify-between text-sm mt-3">
                      <button @click="task.editing = true" class="text-blue-500 text-md bg-blue-100 hover:bg-blue-200 p-1">Edit</button>
                      <button @click="deleteTask(task.id)" class="text-red-500 text-md hover:text-red-600 bg-red-100 hover:bg-red-200 p-1">Delete</button>
                    </div>
                  </div>
                  <div v-else>
                    <input
                      v-model="task.title"
                      class="w-full p-2 border text-lg"
                      @keyup.enter="updateTask(task)"
                      @blur="updateTask(task)"
                    />
                  </div>
                </div>
              </template>
            </draggable>
          </div>
        </template>
      </draggable>
  
      <!-- Add New Task -->
      <div class="mt-4">
        <input v-model="newTask.title" placeholder="New task title" class="border text-lg p-2 w-[400px]" />
        <select v-model="newTask.status" class="border p-2.5 ml-2">
          <option v-for="s in statuses" :key="s" :value="s">{{ s }}</option>
        </select>
        <button @click="addTask" class="bg-blue-500 hover:bg-blue-600 text-white px-3 w-[100px] py-2.5 ml-2">Add</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, reactive } from 'vue'
  import { router } from '@inertiajs/vue3'
  import draggable from 'vuedraggable'
  
  const props = defineProps({ tasks: Array })
  
  const statuses = ['todo', 'in-progress', 'done']
  
  // Reactive column structure
  const columnOrder = ref(
    statuses.map(status => ({
      status,
      tasks: props.tasks
        .filter(task => task.status === status)
        .map(task => ({ ...task, editing: false }))
    }))
  )
  
  const refreshColumns = () => {
    columnOrder.value = statuses.map(status => ({
      status,
      tasks: props.tasks
        .filter(task => task.status === status)
        .map(task => ({ ...task, editing: false }))
    }))
  }
  
  const onDragEnd = (evt, newStatus) => {
    const task = evt.item._underlying_vm_
    router.put(`/tasks/${task.id}`, { ...task, status: newStatus }, { preserveScroll: true })
  }
  
  const newTask = ref({ title: '', status: 'todo' })
  
  const addTask = () => {
    if (newTask.value.title) {
      router.post('/tasks', newTask.value, {
        onSuccess: () => {
          window.location.reload() // force reload to reflect new task
        },
      })
      newTask.value = { title: '', status: 'todo' }
    }
  }
  
  const deleteTask = (id) => {
    router.delete(`/tasks/${id}`, {
      onSuccess: () => {
        window.location.reload()
      },
    })
  }
  
  const updateTask = (task) => {
    task.editing = false
    router.put(`/tasks/${task.id}`, { title: task.title, status: task.status })
  }

  const onTaskChange = (event, newStatus) => {
    if (event.added) {
      const addedTask = event.added.element
      if (addedTask.status !== newStatus) {
        addedTask.status = newStatus
        updateTask(addedTask)
      }
    }
  }
  </script>
  