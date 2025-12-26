<script setup lang="ts">
import { ref, onMounted, nextTick, watch } from 'vue'

const emit = defineEmits(['begin'])

const showText = ref(false)
const isPressed = ref(false)
const pressProgress = ref(0)
const contentHeight = ref<number | null>(null)
const contentRef = ref<HTMLElement | null>(null)
const textRef = ref<HTMLElement | null>(null)
const textHeight = ref(0)
let pressTimer: number | null = null
let progressInterval: number | null = null

const PRESS_DURATION = 2000 // 2 seconds hold

const updateHeight = () => {
  if (contentRef.value) {
    contentHeight.value = contentRef.value.offsetHeight
  }
  if (textRef.value) {
    textHeight.value = textRef.value.offsetHeight
  }
}

onMounted(() => {
  nextTick(() => {
    updateHeight()
  })
  
  setTimeout(() => {
    showText.value = true
  }, 2000)
})

watch(showText, () => {
  nextTick(updateHeight)
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
    <div class="content-wrapper" :style="contentHeight !== null ? { height: contentHeight + 'px' } : {}">
      <div class="content" ref="contentRef">
        <div 
          class="dot-container" 
          :style="showText && textHeight > 0 ? { transform: `translateY(-${textHeight / 2}px)` } : {}"
        >
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
        </div>
        
        <transition name="fade-text">
          <div v-if="showText" class="text-content" ref="textRef">
            <p class="opening-text">
              you look tired.<br />
              let's find your echo.
            </p>
            
            <p class="instruction-text">
              hold to begin
            </p>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<style scoped>
.opening {
  width: 100vw;
  min-height: 100vh;
  background: var(--linen-white);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow-y: auto;
  /* Hide scrollbar */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.opening::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.content-wrapper {
  width: 100%;
  transition: height 1.5s ease;
  position: relative;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  position: relative;
}

.dot-container {
  transition: transform 1.5s ease;
}

.text-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 3rem;
  width: 100%;
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

.text-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3rem;
}

.opening-text {
  font-size: 1.5rem;
  font-weight: 100;
  text-align: center;
  line-height: 2.2;
  color: var(--grave-gray);
  opacity: 0.9;
}

.instruction-text {
  font-size: 0.75rem;
  font-weight: 200;
  text-align: center;
  color: var(--grave-gray);
  opacity: 0.5;
  margin-top: 2rem;
}

.fade-text-enter-active {
  transition: opacity 2s ease;
}

.fade-text-enter-from {
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
