<script setup lang="ts">
import { retrieveLaunchParams, type LaunchParams } from '@tma.js/sdk'
import { ref } from 'vue'

const launchParams = ref<LaunchParams | undefined>()
const launchParamsError = ref()

try {
  launchParams.value = retrieveLaunchParams()
} catch (error) {
  launchParamsError.value = error
}
</script>

<template>
  <header>
    <h1>Init params</h1>
    <pre>{{ JSON.stringify(launchParams?.initData, null, 2) }}</pre>

    <div v-if="launchParamsError">
      <h1>Init params error</h1>
      <pre>{{ launchParamsError }}</pre>
    </div>
  </header>
</template>
