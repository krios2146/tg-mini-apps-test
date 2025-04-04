<script setup lang="ts">
import { retrieveLaunchParams, type LaunchParams } from '@tma.js/sdk'
import { ref } from 'vue'

const launchParams = ref<LaunchParams | undefined>()
const launchParamsError = ref<string>()
const inputText = ref('')

try {
  launchParams.value = retrieveLaunchParams()
} catch (error) {
  launchParamsError.value = error.toString().trim()
}

const copyInitData = async () => {
  try {
    await navigator.clipboard.writeText(JSON.stringify(launchParams.value?.initData, null, 2))
  } catch (err) {
    console.error('Failed to copy:', err)
  }
}

const pasteFromClipboard = async () => {
  try {
    inputText.value = await navigator.clipboard.readText()
  } catch (err) {
    console.error('Failed to paste:', err)
  }
}

const shareText = () => {
  window.Telegram.WebApp.shareMessage(inputText.value)
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100 p-6">
    <div class="bg-white shadow-lg rounded-lg p-6 w-full max-w-2xl">
      <h1 class="text-2xl font-bold text-gray-800 mb-4">Init Params</h1>
      <pre class="bg-gray-200 p-4 rounded-md text-sm text-gray-700 overflow-auto">
        {{ JSON.stringify(launchParams?.initData, null, 2)?.trim() }}
      </pre>

      <div class="flex gap-4 mt-4">
        <button
          @click="copyInitData"
          class="bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded-md transition"
        >
          Copy
        </button>
      </div>

      <div v-if="launchParamsError" class="mt-6">
        <h1 class="text-xl font-bold text-red-600">Init Params Error</h1>
        <p class="bg-red-100 p-3 rounded-md mt-3 text-sm text-red-700 overflow-auto">
          {{ launchParamsError.trim() }}
        </p>
      </div>

      <div class="mt-6">
        <h1 class="text-2xl font-bold text-gray-800 mb-4">Share message</h1>

        <label class="block text-gray-700 font-semibold mb-2">Message ID</label>
        <div class="flex gap-2">
          <input
            v-model="inputText"
            type="text"
            class="border border-gray-300 rounded-md p-2 flex-1 focus:ring focus:ring-blue-300 outline-none"
          />

          <button
            @click="pasteFromClipboard"
            class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-md transition"
          >
            Paste
          </button>
        </div>

        <button
          @click="shareText"
          class="mt-4 w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 rounded-md transition"
        >
          Share
        </button>
      </div>
    </div>
  </div>
</template>
