<script setup>
import { ref } from 'vue'
import Grid from '../Grid.vue'
import { gymHealthFacts } from '../../utils'

const props = defineProps({
    handleSelectWorkout: Function,
    firstIncompleteWorkoutIndex: Number,
})

// (show-facts): Function to generate a random fact
function getRandomFact() {
  const randomIndex = Math.floor(Math.random() * gymHealthFacts.length)
  return gymHealthFacts[randomIndex]
}

// (show-facts): Store the current fact in a reactive ref
const currentFact = ref(getRandomFact())

// (show-facts): Change the fact when the button is clicked
function showAnotherFact() {
  let newFact = getRandomFact()
  // Prevent showing the same fact
  while (newFact === currentFact.value && gymHealthFacts.length > 1) {
    newFact = getRandomFact()
  }
  currentFact.value = newFact
}
</script>

<template>
    <section id="dashboard">
        <div class="card tip-container">
            <h2>Welcome to Workout</h2>
        </div>

        <div class="fact-box">
            <p class="tip">
                <strong>Daily Tip</strong><br />
                {{ currentFact }}
            </p>

            <button @click="showAnotherFact" class="reload-btn">
                another :)
            </button>

            <button @click="() => handleSelectWorkout(
                firstIncompleteWorkoutIndex < 0 ? 0 : firstIncompleteWorkoutIndex)"
                >Start workout &rarr;
            </button>
        </div>

        <Grid v-bind="props"  />
    </section>
</template>

<style scoped>
    .tip-container,
    .tip-container div,
    #dashboard {
        display: flex;
    }

    .tip-container,
    #dashboard {
        flex-direction: column;
    }

    #dashboard {
        gap: 2rem;
    }

    .tip-container {
        gap: 0.5rem;
    }

    /* FACT BOX (show-facts) */
    .fact-box {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 1rem;
      border: 1px solid #ccc;
      border-radius: 8px;
      position: relative;
      background-color: #f9f9f9;
    }

    .tip {
      margin: 0;
      font-size: 1rem;
      line-height: 1.4;
      color: #333;
    }

    .reload-btn {
      align-self: flex-end;
      margin-top: .5rem;
      margin-bottom: 1rem;
      background: transparent;
      border: 1px solid #ccc;
      cursor: pointer;
      font-weight: bold;
      font-size: small;
    }

    .reload-btn:hover {
      color: #313131;
    }

    @media (min-width: 640px) {
        .tip-container {
            gap: 1rem;
        }
    }
</style>
