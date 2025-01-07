<script setup lang="ts">
import { ref } from 'vue';
import Layout from './components/layouts/Layout.vue';
import Welcome from './components/pages/Welcome.vue';
import Dashboard from './components/pages/Dashboard.vue';

const selectedDisplay = ref(1)

function handleChangeDisplay(idx: number) {
  selectedDisplay.value = idx
}

</script>

<template>
  <Layout>
    <Welcome 
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay == 1"
    />

    <Dashboard
      v-if="selectedDisplay == 2"
    />
  </Layout>
</template>

<style scoped>
</style>
