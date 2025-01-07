<script setup lang="ts">
import { ref } from 'vue';
import Layout from './components/layouts/Layout.vue';
import Welcome from './components/pages/Welcome.vue';

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
  </Layout>
</template>

<style scoped>
</style>
