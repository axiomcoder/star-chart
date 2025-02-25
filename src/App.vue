<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue'
import StarTable from './components/StarTable.vue'
import StickerChart from './components/StickerChart.vue'
import { ref } from 'vue'

const myTasks = ref([
  { id: 1, name: 'Morning Exercise', description: '30 minutes of cardio' },
  { id: 2, name: 'Read a Book', description: 'At least 20 pages' },
  { id: 3, name: 'Drink Water', description: '8 glasses' },
])

const myTaskStates = ref({
  1: [false, true, false, true, false, false, false],
  2: [false, false, false, false, false, true, true],
  3: [true, true, true, true, true, true, true],
})

function handleTaskToggle({ task, dayIndex, day, newState }) {
  myTaskStates.value[task.id][dayIndex] = newState
  console.log(`Task "${task.name}" on ${day} set to ${newState ? 'completed' : 'not completed'}`)
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/star.png" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <StickerChart :tasks="myTasks" :taskStates="myTaskStates" @taskToggled="handleTaskToggle" />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
