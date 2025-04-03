<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'

const text = ref('')
const voices = ref<SpeechSynthesisVoice[]>([])
const selectedVoice = ref<SpeechSynthesisVoice | null>(null)
const isReading = ref(false)
const isPaused = ref(false)

// 语速和音调控制
const speechRate = ref(1)
const pitch = ref(1)

// 格式化显示值
const formattedSpeechRate = computed(() => speechRate.value.toFixed(1))
const formattedPitch = computed(() => pitch.value.toFixed(1))

const features = [
  {
    title: '智能语音阅读',
    description: '支持多种语音引擎，提供自然流畅的阅读体验。',
    icon: 'i-carbon-ibm-watson-speech-to-text',
  },
  {
    title: '多语言支持',
    description: '支持中文、英文等多种语言的文本阅读。',
    icon: 'i-carbon-language',
  },
  {
    title: '阅读控制',
    description: '可调节语速、音调，支持暂停、继续等操作。',
    icon: 'i-carbon-play',
  },
]

function getVoiceName(voice: SpeechSynthesisVoice) {
  const languageMap: Record<string, string> = {
    'zh-CN': '简体中文',
    'zh-TW': '繁体中文',
    'zh-HK': '粤语',
    'zh-CN-liaoning': '东北普通话',
    'zh-CN-shaanxi': '陕西中原官话',
  }
  const language = languageMap[voice.lang] || voice.lang
  return `${voice.name}（${language}）`
}

function loadVoices() {
  setTimeout(() => {
    const allVoices = speechSynthesis.getVoices()
    voices.value = allVoices.filter(voice => voice.lang.includes('zh'))
    selectedVoice.value = voices.value[0] || null
  }, 100)
}

function startReading() {
  if (!text.value)
    return

  const utterance = new SpeechSynthesisUtterance(text.value)
  utterance.voice = selectedVoice.value
  utterance.rate = speechRate.value
  utterance.pitch = pitch.value

  isReading.value = true
  isPaused.value = false
  speechSynthesis.speak(utterance)

  utterance.onend = () => {
    isReading.value = false
    isPaused.value = false
  }
}

function pauseReading() {
  if (isReading.value) {
    speechSynthesis.pause()
    isPaused.value = true
  }
}

function resumeReading() {
  if (isPaused.value) {
    speechSynthesis.resume()
    isPaused.value = false
  }
}

function stopReading() {
  speechSynthesis.cancel()
  isReading.value = false
  isPaused.value = false
}

function clearText() {
  text.value = ''
  stopReading()
}

onMounted(() => {
  loadVoices()
  speechSynthesis.onvoiceschanged = loadVoices
})
</script>

<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <!-- Hero Section -->
    <section class="px-4 py-6 text-center sm:py-8">
      <h1 class="mb-2 text-3xl text-gray-900 font-bold sm:mb-4 sm:text-4xl dark:text-white">
        智能语音阅读助手
      </h1>
      <p class="mx-auto max-w-2xl text-base text-gray-600 sm:text-lg dark:text-gray-300">
        让文字发声，让阅读更轻松
      </p>
    </section>

    <!-- Main Content -->
    <div class="mx-auto max-w-4xl px-4 py-4 sm:py-8">
      <!-- Reading Area -->
      <section class="rounded-lg bg-white p-4 shadow-sm dark:bg-gray-800 sm:p-6">
        <div class="mb-4">
          <select
            v-model="selectedVoice"
            class="w-full border border-gray-300 rounded-md bg-white px-3 py-2 text-sm text-gray-700 dark:border-gray-600 dark:bg-gray-700 sm:text-base dark:text-gray-300"
          >
            <option v-for="voice in voices" :key="voice.name" :value="voice">
              {{ getVoiceName(voice) }}
            </option>
          </select>
        </div>

        <!-- Speech Controls -->
        <div class="grid grid-cols-1 mb-4 gap-4 sm:grid-cols-2">
          <div>
            <label class="mb-1 block text-sm text-gray-700 font-medium dark:text-gray-300">
              语速: {{ formattedSpeechRate }}x
            </label>
            <input
              v-model.number="speechRate"
              type="range"
              min="0.5"
              max="2"
              step="0.1"
              class="h-2 w-full cursor-pointer appearance-none rounded-lg bg-gray-200 dark:bg-gray-700"
            >
          </div>
          <div>
            <label class="mb-1 block text-sm text-gray-700 font-medium dark:text-gray-300">
              音调: {{ formattedPitch }}x
            </label>
            <input
              v-model.number="pitch"
              type="range"
              min="0.5"
              max="2"
              step="0.1"
              class="h-2 w-full cursor-pointer appearance-none rounded-lg bg-gray-200 dark:bg-gray-700"
            >
          </div>
        </div>

        <textarea
          v-model="text"
          class="min-h-[200px] w-full resize-none rounded-lg bg-gray-50 p-4 text-sm text-gray-600 outline-none sm:min-h-[300px] dark:bg-gray-700 sm:text-base dark:text-gray-300"
          placeholder="在这里粘贴或输入要阅读的文本..."
        />

        <div class="mt-4 flex flex-wrap justify-end gap-2 sm:space-x-4">
          <button
            class="flex-1 rounded-md bg-gray-100 px-4 py-2 text-sm text-gray-700 font-medium sm:flex-none dark:bg-gray-700 hover:bg-gray-200 dark:text-gray-300"
            @click="clearText"
          >
            清除
          </button>
          <template v-if="!isReading">
            <button
              class="flex-1 rounded-md bg-indigo-600 px-4 py-2 text-sm text-white font-medium sm:flex-none hover:bg-indigo-700"
              @click="startReading"
            >
              开始阅读
            </button>
          </template>
          <template v-else>
            <button
              v-if="!isPaused"
              class="flex-1 rounded-md bg-yellow-600 px-4 py-2 text-sm text-white font-medium sm:flex-none hover:bg-yellow-700"
              @click="pauseReading"
            >
              暂停
            </button>
            <button
              v-else
              class="flex-1 rounded-md bg-green-600 px-4 py-2 text-sm text-white font-medium sm:flex-none hover:bg-green-700"
              @click="resumeReading"
            >
              继续
            </button>
            <button
              class="flex-1 rounded-md bg-red-600 px-4 py-2 text-sm text-white font-medium sm:flex-none hover:bg-red-700"
              @click="stopReading"
            >
              停止
            </button>
          </template>
        </div>
      </section>

      <!-- Features Section -->
      <section class="mt-6 sm:mt-8">
        <div class="grid grid-cols-1 gap-4 sm:grid-cols-3 sm:gap-6">
          <div
            v-for="feature in features"
            :key="feature.title"
            class="rounded-lg bg-white p-4 shadow-sm transition-shadow dark:bg-gray-800 sm:p-6 hover:shadow-md"
          >
            <div class="mb-3 h-10 w-10 flex items-center justify-center rounded-md bg-indigo-500 text-white sm:mb-4 sm:h-12 sm:w-12">
              <span :class="feature.icon" class="text-xl sm:text-2xl" />
            </div>
            <h3 class="mb-2 text-base text-gray-900 font-medium sm:text-lg dark:text-white">
              {{ feature.title }}
            </h3>
            <p class="text-sm text-gray-500 sm:text-base dark:text-gray-400">
              {{ feature.description }}
            </p>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>
