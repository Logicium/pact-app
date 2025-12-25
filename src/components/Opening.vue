<script setup lang="ts">
import { ref, onMounted } from 'vue'

const emit = defineEmits(['begin'])

const showText = ref(false)
const isPressed = ref(false)
const pressProgress = ref(0)
let pressTimer: number | null = null
let progressInterval: number | null = null

const PRESS_DURATION = 2000 // 2 seconds hold

onMounted(() => {
  setTimeout(() => {
    showText.value = true
  }, 2000)
})

const startPress = () => {
  isPressed.value = true
  pressProgress.value = 0
  
  const startTime = Date.now()
  
  progressInterval = window.setInterval(() => {
    const elapsed = Date.now() - startTime
    pressProgress.value = Math.min((elapsed / PRESS_DURATION) * 100, 100)
  }, 16)
  
  pressTimer = window.setTimeout(() => {
    emit('begin')
    if (progressInterval) clearInterval(progressInterval)
  }, PRESS_DURATION)
}

const endPress = () => {
  isPressed.value = false
  pressProgress.value = 0
  if (pressTimer) clearTimeout(pressTimer)
  if (progressInterval) clearInterval(progressInterval)
}
</script>

<template>
  <div class="opening">
    <div class="content">
      <div 
        class="amber-dot"
        :class="{ pressed: isPressed }"
        @mousedown="startPress"
        @mouseup="endPress"
        @mouseleave="endPress"
        @touchstart.prevent="startPress"
        @touchend.prevent="endPress"
      >
        <div class="progress-ring" :style="{ '--progress': pressProgress }"></div>
      </div>
      
      <transition name="fade-text">
        <p v-if="showText" class="opening-text">
          you look tired.<br />
          let's find your echo.
        </p>
      </transition>
      
      <transition name="fade-text">
        <p v-if="showText" class="instruction-text">
          hold to begin
        </p>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.opening {
  width: 100vw;
  height: 100vh;
  background: #000000;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3rem;
}

.amber-dot {
  width: 60px;
  height: 60px;
  background: var(--sunset-amber);
  border-radius: 50%;
  cursor: pointer;
  animation: pulse 3s ease-in-out infinite;
  position: relative;
  transition: all var(--transition-slow);
  box-shadow: 0 0 30px rgba(255, 157, 0, 0.4);
}

.amber-dot.pressed {
  animation: none;
  transform: scale(1.1);
  box-shadow: 0 0 50px rgba(255, 157, 0, 0.8);
}

.progress-ring {
  position: absolute;
  top: -5px;
  left: -5px;
  right: -5px;
  bottom: -5px;
  border-radius: 50%;
  background: conic-gradient(
    var(--sunset-amber) calc(var(--progress) * 1%),
    transparent calc(var(--progress) * 1%)
  );
  opacity: 0.6;
  transition: opacity 0.3s;
}

.amber-dot:not(.pressed) .progress-ring {
  opacity: 0;
}

.opening-text {
  font-size: 1.5rem;
  font-weight: 100;
  text-align: center;
  line-height: 2.2;
  color: var(--linen-white);
  opacity: 0.9;
}

.instruction-text {
  font-size: 0.75rem;
  font-weight: 200;
  text-align: center;
  color: var(--linen-white);
  opacity: 0.5;
  margin-top: 2rem;
}

.fade-text-enter-active {
  animation: fadeIn 3s ease-out;
}

.fade-text-leave-active {
  transition: opacity var(--transition-slow);
}

.fade-text-enter-from,
.fade-text-leave-to {
  opacity: 0;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 0.6;
    box-shadow: 0 0 30px rgba(255, 157, 0, 0.4);
  }
  50% {
    transform: scale(1.15);
    opacity: 1;
    box-shadow: 0 0 50px rgba(255, 157, 0, 0.7);
  }
}
</style>
