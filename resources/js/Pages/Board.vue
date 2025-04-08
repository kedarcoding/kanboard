<template>
  <div class="p-4 grid grid-cols-3 gap-4">
    <Column v-for="(columnTasks, title) in tasks" :key="title" :title="title" :tasks="columnTasks" @update:tasks="updateColumnTasks(title, $event)" />
  </div>
</template>

<script setup>
import Column from './Partials/Column.vue'
import { reactive } from 'vue'

const props = defineProps({
  tasks: Object,
})

const tasks = reactive({ ...props.tasks })

function updateColumnTasks(column, newTasks) {
  tasks[column] = newTasks
}
</script>