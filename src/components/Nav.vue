<script setup lang="ts">
import { useDark, useToggle } from '@vueuse/core'

const isDark = useDark()
const toggleDark = useToggle(isDark)
</script>

<template>
  <nav class="fixed left-0 right-0 top-0 z-50 bg-white shadow-sm dark:bg-gray-800">
    <div class="mx-auto max-w-7xl px-4 lg:px-8 sm:px-6">
      <div class="h-16 flex justify-between">
        <div class="flex items-center">
          <div class="flex flex-shrink-0 items-center">
            <span class="text-2xl text-indigo-600 font-bold dark:text-indigo-400">语音阅读助手</span>
          </div>
        </div>
        <div class="flex items-center space-x-4">
          <button
            class="i-carbon-sun dark:i-carbon-moon rounded-full p-2 transition-colors hover:bg-indigo-700"
            @click="toggleDark()"
          />
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.router-link-active {
  background-color: rgb(224 231 255);
  color: rgb(67 56 202);
}
.dark .router-link-active {
  background-color: rgb(49 46 129);
  color: rgb(199 210 254);
}
</style>
