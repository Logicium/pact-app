<script setup lang="ts">
import { ref } from 'vue'
import HomePage from './components/HomePage.vue'
import Opening from './components/Opening.vue'
import Quiz from './components/Quiz.vue'
import Matching from './components/Matching.vue'
import Dashboard from './components/Dashboard.vue'
import TerminalHandshake from './components/TerminalHandshake.vue'

type AppState = 'home' | 'opening' | 'quiz' | 'matching' | 'dashboard' | 'terminal'

const currentState = ref<AppState>('home')
const quizAnswers = ref({
  nickname: '',
  soundOfWorld: '',
  geometry: '',
  coreAbsence: '',
  surprise: 50,
  physicality: ''
})

const startExperience = () => {
  currentState.value = 'opening'
}

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

const initiateTerminal = () => {
  currentState.value = 'terminal'
}
</script>

<template>
  <div class="app-container">
    <HomePage v-if="currentState === 'home'" @start="startExperience" />
    <Opening v-else-if="currentState === 'opening'" @begin="advanceToQuiz" />
    <Quiz v-else-if="currentState === 'quiz'" @complete="completeQuiz" />
    <Matching v-else-if="currentState === 'matching'" @matched="advanceToDashboard" :answers="quizAnswers" />
    <Dashboard v-else-if="currentState === 'dashboard'" @initiate="initiateTerminal" :answers="quizAnswers" />
    <TerminalHandshake v-else-if="currentState === 'terminal'" />
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
  background: var(--linen-white);
  color: var(--grave-gray);
  letter-spacing: 0.15em;
  overflow-x: hidden;
  overflow-y: auto;
  text-transform: lowercase;
  /* Hide scrollbar for Chrome, Safari and Opera */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

body::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.app-container {
  width: 100vw;
  min-height: 100vh;
  overflow-x: hidden;
  overflow-y: auto;
  animation: breathe 8s ease-in-out infinite;
  /* Hide scrollbar */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.app-container::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
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
