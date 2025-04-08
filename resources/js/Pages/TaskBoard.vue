<template>
    <div class="p-4">
      <!-- Draggable Columns -->
      <draggable v-model="columnOrder" item-key="status" class="flex gap-4" group="columns" direction="horizontal">
        <template #item="{ element: column }">
          <div class="w-1/3 border p-2">
            <h2 class="text-lg font-bold capitalize mb-2">{{ column.status }}</h2>
  
            <!-- Draggable Tasks -->
            <draggable
              v-model="column.tasks"
              group="tasks"
              @end="(e) => onDragEnd(e, column.status)"
              item-key="id"
            >
              <template #item="{ element: task }">
                <div class="bg-gray-100 p-2 my-2 rounded">
                  <div v-if="!task.editing">
                    {{ task.title }}
                    <div class="flex justify-between text-sm mt-1">
                      <button @click="task.editing = true" class="text-blue-500">Edit</button>
                      <button @click="deleteTask(task.id)" class="text-red-500">Delete</button>
                    </div>
                  </div>
                  <div v-else>
                    <input
                      v-model="task.title"
                      class="w-full p-1 border"
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
        <input v-model="newTask.title" placeholder="New task title" class="border p-1" />
        <select v-model="newTask.status" class="border p-1 ml-2">
          <option v-for="s in statuses" :key="s" :value="s">{{ s }}</option>
        </select>
        <button @click="addTask" class="bg-blue-500 text-white px-3 py-1 ml-2">Add</button>
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
  </script>
  