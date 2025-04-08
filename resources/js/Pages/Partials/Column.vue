<template>
    <div class="w-full max-w-sm bg-white rounded-2xl shadow p-4">
      <h2 class="text-lg font-bold text-center mb-4">{{ title }}</h2>
  
      <Draggable
        :list="tasks"
        group="tasks"
        itemKey="id"
        class="space-y-2"
        @update:list="onUpdate"
      >
        <template #item="{ element }">
          <div class="p-3 bg-gray-100 rounded shadow-sm">
            {{ element.title }}
          </div>
        </template>
      </Draggable>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits } from 'vue'
  import { Draggable } from 'vue-draggable-next'
  
  const props = defineProps({
    title: String,
    tasks: Array
  })
  
  const emits = defineEmits(['update:tasks'])
  
  const onUpdate = (newList) => {
    emits('update:tasks', newList)
  }
  </script>
  