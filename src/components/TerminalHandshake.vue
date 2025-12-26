<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isFading = ref(false)
const showFinalText = ref(false)

onMounted(() => {
  // Auto-start the fade after a brief moment
  setTimeout(() => {
    initiateTransition()
  }, 1000)
})

const initiateTransition = () => {
  isFading.value = true
  
  // Show final text after fade completes
  setTimeout(() => {
    showFinalText.value = true
  }, 5000)
}
</script>

<template>
  <div class="terminal" :class="{ fading: isFading }">
    <div class="terminal-content">
      <transition name="fade-text">
        <p v-if="showFinalText" class="final-text">
          rest now. we are holding your hand.
        </p>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.terminal {
  width: 100vw;
  min-height: 100vh;
  background: #e8e2d9;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 5s ease-out;
  overflow-y: auto;
  /* Hide scrollbar */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.terminal::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.terminal.fading {
  background: #1a1a1b;
}

.terminal-content {
  max-width: 600px;
  width: 100%;
  text-align: center;
  padding: 2rem;
}

.final-text {
  font-size: 1.5rem;
  font-weight: 100;
  letter-spacing: 0.2em;
  line-height: 2;
  color: #e8e2d9;
  opacity: 0.9;
}

.fade-text-enter-active {
  transition: opacity 3s ease-out;
}

.fade-text-enter-from {
  opacity: 0;
}

.fade-text-enter-to {
  opacity: 0.9;
}
</style>
