<script setup lang="ts">
import { ref } from 'vue'
import Opening from './components/Opening.vue'
import Quiz from './components/Quiz.vue'
import Matching from './components/Matching.vue'
import Dashboard from './components/Dashboard.vue'

type AppState = 'opening' | 'quiz' | 'matching' | 'dashboard'

const currentState = ref<AppState>('opening')
const quizAnswers = ref({
  soundOfWorld: '',
  geometry: '',
  surprise: 50,
  physicality: ''
})

const advanceToQuiz = () => {
  currentState.value = 'quiz'
}

const completeQuiz = (answers: typeof quizAnswers.value) => {
  quizAnswers.value = answers
  currentState.value = 'matching'
}

const advanceToDashboard = () => {
  currentState.value = 'dashboard'
}
</script>

<template>
  <div class="app-container">
    <Opening v-if="currentState === 'opening'" @begin="advanceToQuiz" />
    <Quiz v-else-if="currentState === 'quiz'" @complete="completeQuiz" />
    <Matching v-else-if="currentState === 'matching'" @matched="advanceToDashboard" :answers="quizAnswers" />
    <Dashboard v-else-if="currentState === 'dashboard'" :answers="quizAnswers" />
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100;200;300;400&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --grave-gray: #1a1a1b;
  --linen-white: #e8e2d9;
  --sunset-amber: #ff9d00;
  --transition-slow: 800ms;
  --transition-breath: 1200ms;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  font-weight: 200;
  background: var(--grave-gray);
  color: var(--linen-white);
  letter-spacing: 0.15em;
  overflow: hidden;
  text-transform: lowercase;
}

.app-container {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  animation: breathe 8s ease-in-out infinite;
}

@keyframes breathe {
  0%, 100% {
    transform: scale(1);
    opacity: 0.98;
  }
  50% {
    transform: scale(1.005);
    opacity: 1;
  }
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 0.6;
  }
  50% {
    transform: scale(1.3);
    opacity: 1;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity var(--transition-slow);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
