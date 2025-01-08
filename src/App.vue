<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';
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

const isWorkoutComplete = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value]
  if (!currWorkout) { return false } // guard clause to exit function

  const isCompleteCheck = Object.values(currWorkout).every(ex => !!ex)
  console.log('ISCOMPLETE: ', isCompleteCheck)
  return isCompleteCheck
})

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

function handleSaveWorkout() {
  // save the current data snapshot to localstorage so that we can retrieve it next time
  localStorage.setItem('workouts', JSON.stringify(data.value))

  // show the dashboard
  selectedDisplay.value = 2

  // deselect a workout
  selectedWorkout.value = -1
}

function handleResetPlan() {
  selectedDisplay.value = 2
  selectedWorkout.value = -1
  data.value = defaultData
  localStorage.removeItem('workouts')
}

onMounted(() => {
  if (!localStorage) {return}
  const raw = localStorage.getItem('workouts')
  if (raw) {
    // only enter the if block if we find some data saved to the key workouts in localstroage database
    const savedData = JSON.parse(raw)
    data.value = savedData
    selectedDisplay.value = 2 // if they have data, then we dont want them landing on the welcome screen every time they enter the app
  }
})

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
      :handleResetPlan="handleResetPlan"
      v-if="selectedDisplay == 2"
    />

    <Workout
      :data="data" 
      :selectedWorkout="selectedWorkout"
      :handleSaveWorkout="handleSaveWorkout"
      :isWorkoutComplete="isWorkoutComplete"
      :handleChangeDisplay="handleChangeDisplay"
      v-if="workoutProgram?.[selectedWorkout] && selectedDisplay == 3"
    />
  </Layout>
</template>

<style scoped>
</style>
