<script setup lang="ts">
import { computed, ref } from 'vue';
import Layout from './components/layouts/Layout.vue';
import Welcome from './components/pages/Welcome.vue';
import Dashboard from './components/pages/Dashboard.vue';
import Workout from './components/pages/Workout.vue';
import { workoutProgram, type WorkoutProgram } from './utils';

// This is the shape of each day's tracking data
type WorkoutTrackingData = Record<string, string> // { [exerciseName]: string }

// All days’ tracking data
type TrackingState = Record<number, WorkoutTrackingData>

const selectedDisplay = ref(1)
const selectedWorkout = ref(-1)

// Initialize empty Workout Tracking State from Workout Program
const defaultData: TrackingState = {}
// Create a structured object to track user input for each workout exercise, 
for (const [key, workoutData] of Object.entries(workoutProgram as WorkoutProgram)) {
  const dayIndex = Number(key)
  defaultData[dayIndex] = {}

  for (const e of workoutData.workout) {
    defaultData[dayIndex][e.name] = ''
  }
}

const data = ref<TrackingState>(defaultData) // {1....30: {exercise_name: '', .....}}

// Computed property to find first incomplete workout index
// It scanning through the data to find the first day where at least one exercise hasn't been filled in.
const firstIncompleteWorkoutIndex = computed(() => {
  const allWorkouts = data.value

  // loop over every key value pair, and check if the workout is complete or not
  for (const [index, workout] of Object.entries(allWorkouts)) {
    // Are all values in the workout truthy?
    const isComplete = Object.values(workout).every(ex => !!ex)

    if (!isComplete) {
      return parseInt(index)
    }
  }
  return -1 // all are complete
})

function handleChangeDisplay(idx: number) {
  selectedDisplay.value = idx
}

function handleSelectWorkout(idx: number) {
  selectedDisplay.value = 3
  selectedWorkout.value = idx
}

</script>

<template>
  <Layout>
    <Welcome 
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay == 1"
    />

    <Dashboard
      :handleSelectWorkout="handleSelectWorkout"
      :firstIncompleteWorkoutIndex="firstIncompleteWorkoutIndex"
      v-if="selectedDisplay == 2"
    />

    <Workout
      :data="data" 
      :selectedWorkout="selectedWorkout"
      :handleChangeDisplay="handleChangeDisplay"
      v-if="workoutProgram?.[selectedWorkout] && selectedDisplay == 3"
    />
  </Layout>
</template>

<style scoped>
</style>
