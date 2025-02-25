<script setup>
// Define props for the component
const props = defineProps({
  tasks: {
    type: Array,
    required: true,
  },
  taskStates: {
    type: Object,
    required: true,
  },
})

// Define emits for the component
const emit = defineEmits(['taskToggled'])

// Days of the week (Monday to Sunday)
const daysOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

// Function to toggle a task state for a specific day
function toggleTask(task, dayIndex) {
  // Get the current state
  const currentState = (props.taskStates[task.id] && props.taskStates[task.id][dayIndex]) || false

  // Emit the event with the task and day
  emit('taskToggled', {
    task,
    dayIndex,
    day: daysOfWeek[dayIndex],
    newState: !currentState,
  })
}
</script>

<template>
  <div class="sticker-chart">
    <table>
      <thead>
        <tr>
          <th>Task</th>
          <th v-for="(day, index) in daysOfWeek" :key="index">{{ day }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td class="task-name">
            {{ task.name }}
            <div v-if="task.description" class="task-description">{{ task.description }}</div>
          </td>
          <td
            v-for="(day, dayIndex) in daysOfWeek"
            :key="dayIndex"
            class="task-cell"
            :class="{ 'task-completed': taskStates[task.id] && taskStates[task.id][dayIndex] }"
            @click="toggleTask(task, dayIndex)"
          >
            <img
              v-if="taskStates[task.id] && taskStates[task.id][dayIndex]"
              src="../assets/star.png"
              height="100px"
              width="100px"
              alt="Star"
              class="star-image"
            />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.sticker-chart {
  margin: 20px;
  font-family: Arial, sans-serif;
}

table {
  width: 100%;
  border-collapse: collapse;
  /* border: 1px solid #ddd; */
}

th,
td {
  /* border: 1px solid #ddd; */
  padding: 12px;
  text-align: center;
}

th {
  /* background-color: #f2f2f2; */
  font-weight: bold;
}

.task-name {
  text-align: left;
  font-weight: bold;
  min-width: 150px;
}

.task-description {
  font-size: 0.8em;
  /* color: #666; */
  font-weight: normal;
  margin-top: 4px;
}

.task-cell {
  position: relative;
  min-width: 50px;
  height: 50px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.task-checkbox {
  margin: 0;
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 1;
}

.star-image {
  width: 30px;
  height: 30px;
  display: block;
  margin: 0 auto;
}
</style>
