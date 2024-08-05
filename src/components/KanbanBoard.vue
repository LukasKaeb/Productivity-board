<template>
    <div class="kanban-container flex flex-col items-center p-4">
        <h1 class="p-4 font-bold text-xl">Productivity Board</h1>
        <input v-model="task" @keyup.enter="handleSubmit" class="mb-4 w-full max-w-md px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Enter a task...">

        <div class="kanban-board flex gap-4 p-4">
        <div :class="column.bgColor" class="kanban-column relative w-80 bg-gray-100 rounded-lg p-4" v-for="(column, index) in columns" :key="index">
            <h2 :class="column.headingColor" class="absolute top-0 left-0 right-0 bg-white p-4 border-b border-gray-200 z-10">{{ column.title }}</h2>
            <div class="flex flex-col mt-16 min-h-[300px]">
                <draggable
            v-model="column.items"
            :options="dragOptions"
            group="kanban"
            class="flex-1"
            >
            <template #item="{ element }">
                <div class="kanban-item p-4 cursor-pointer bg-white border border-gray-200 mb-2 rounded">
                    {{ element }}
                </div>
                </template>
                </draggable>
                <!-- Add placeholder for empty column -->
                <div v-if="column.items.length === 0 " class="flex-1 flex items-center justify-center text-gray-400 p-4 text-center border-dashed border-2 border-gray-300">
                    Nothing here yet...
                </div>
            </div>
        </div>
    </div>
    </div>
</template>

<script setup>
import draggable from 'vuedraggable'
import { ref, defineComponent } from 'vue'

const task = ref('')

const columns = ref([
    {
    title: 'Log',
    items: [],
    bgColor: 'bg-pink-100',
    headingColor: 'bg-pink-300',
    },
    {
    title: 'To Do',
    items: [],
    bgColor: 'bg-purple-100',
    headingColor: 'bg-purple-300',
    },
    {
    title: 'In Progress',
    items: [],
    bgColor: 'bg-yellow-100',
    headingColor: 'bg-yellow-300',
    },
    {
    title: 'Done',
    items: [],
    bgColor: 'bg-teal-100',
    headingColor: 'bg-teal-300',
    }
])

// Handle task submit

const handleSubmit = () => {
    if(task.value.trim() === '') return
    columns.value[0].items.push(task.value) // Add task to first column
    task.value = '' // Clear input
}

const dragOptions = {
    group: {
        name: 'kanban',
        pull: 'clone',
        put: true,
    },
    disabled: false,
    fallbackOnBody: true,
}



</script>

<style scoped>
.kanban-column {
  position: relative;
  display: flex;
  flex-direction: column;
  min-height: 300px; /* Ensure the column has enough height */
}

.kanban-item {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 0.5rem;
  margin-bottom: 0.5rem;
  border-radius: 4px;
}
</style>