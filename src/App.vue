<script setup lang="ts">
import { retrieveLaunchParams, type LaunchParams } from '@tma.js/sdk'
import { ref } from 'vue'
import axios, { AxiosError, type AxiosResponse } from 'axios'

const launchParams = ref<LaunchParams | undefined>()
const launchParamsError = ref()
const response = ref<AxiosResponse>()
const error = ref()

try {
  launchParams.value = retrieveLaunchParams()
} catch (error) {
  launchParamsError.value = error
}

const url = import.meta.env.VITE_BACKEND_URL

axios
  .post(`${url}/auth/sign-in/by-telegram`, {
    initDataRaw: launchParams.value?.initDataRaw,
    initData: launchParams.value?.initData
  })
  .then((apiResponse) => (response.value = apiResponse))
  .catch((apiError: AxiosError) => (error.value = apiError))
</script>

<template>
  <header>
    <h1>Init params</h1>
    <pre>{{ JSON.stringify(launchParams?.initData, null, 2) }}</pre>

    <div v-if="launchParamsError">
      <h1>Init params error</h1>
      <pre>{{ launchParamsError }}</pre>
    </div>

    <h1>API Response</h1>
    <pre>{{ JSON.stringify(response, null, 2) }}</pre>

    <div v-if="response?.status === 200">Telegram auth is successful</div>

    <div v-if="error">
      <h1>API Error</h1>
      <pre>{{ JSON.stringify(error, null, 2) }}</pre>
    </div>
  </header>
</template>
