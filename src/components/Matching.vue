<script setup lang="ts">
import { ref, onMounted } from 'vue'

const props = defineProps<{
  answers: {
    nickname: string
    soundOfWorld: string
    geometry: string
    coreAbsence: string
    surprise: number
    physicality: string
  }
}>()

const emit = defineEmits(['matched'])

const isScanning = ref(true)
const matchFound = ref(false)
const showBreathingBtn = ref(false)
const isBreathing = ref(false)
const audioElement = ref<HTMLAudioElement | null>(null)

// Partner nickname (simulated)
const partnerNickname = ref('willow')

onMounted(() => {
  // Simulate scanning for 3 seconds
  setTimeout(() => {
    isScanning.value = false
    matchFound.value = true
    setTimeout(() => {
      showBreathingBtn.value = true
    }, 1000)
  }, 3000)
})

const toggleBreathing = () => {
  isBreathing.value = !isBreathing.value
  
  if (isBreathing.value && audioElement.value) {
    audioElement.value.play()
  } else if (audioElement.value) {
    audioElement.value.pause()
  }
}

const proceedToDashboard = () => {
  if (audioElement.value) {
    audioElement.value.pause()
  }
  emit('matched')
}

// Determine match characteristics based on answers
const getMatchDescription = () => {
  const sound = props.answers.soundOfWorld
  const physical = props.answers.physicality
  const core = props.answers.coreAbsence
  
  return {
    sound: sound === 'static' ? 'static' : sound === 'ringing' ? 'ringing' : 'silence',
    location: physical === 'chest' ? 'chest' : physical === 'neck' ? 'neck' : 'feet',
    absence: core
  }
}

const matchDesc = getMatchDescription()
</script>

<template>
  <div class="matching">
    <div class="matching-container">
      <!-- Scanning State -->
      <transition name="fade">
        <div v-if="isScanning" class="scanning-state">
          <p class="scanning-text">scanning for your echo...</p>
          <svg class="waveform" viewBox="0 0 200 60" xmlns="http://www.w3.org/2000/svg">
            <path 
              class="wave-path"
              d="M 0 30 Q 25 10, 50 30 T 100 30 T 150 30 T 200 30"
              fill="none"
              stroke="var(--sunset-amber)"
              stroke-width="2"
            />
          </svg>
        </div>
      </transition>

      <!-- Match Found State -->
      <transition name="fade">
        <div v-if="matchFound" class="match-found">
          <div class="match-info">
            <p class="match-title">match found</p>
            <p class="match-id">{{ partnerNickname }}</p>
            <p class="match-subtitle">user 771</p>
            <p class="match-description">
              {{ partnerNickname }}'s {{ matchDesc.sound }} sounds like yours.<br />
              they also feel the weight in their {{ matchDesc.location }}.<br />
              they are waiting in the 'quiet room.'
            </p>
          </div>

          <transition name="slide-up">
            <div v-if="showBreathingBtn" class="breathing-section">
              <p class="breathing-prompt">would you like to hear their breathing?</p>
              
              <button 
                class="breathe-btn"
                :class="{ active: isBreathing }"
                @click="toggleBreathing"
              >
                <span class="breathe-circle"></span>
                {{ isBreathing ? 'listening...' : 'breathe with them' }}
              </button>

              <audio ref="audioElement" loop>
                <source src="/breathing.mp3" type="audio/mpeg">
                <!-- Fallback: The audio will be a placeholder; in production you'd have actual breathing audio -->
              </audio>

              <transition name="fade">
                <button 
                  v-if="isBreathing" 
                  class="continue-btn"
                  @click="proceedToDashboard"
                >
                  enter the quiet room
                </button>
              </transition>
            </div>
          </transition>
        </div>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.matching {
  width: 100vw;
  min-height: 100vh;
  background: var(--linen-white);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  overflow-y: auto;
  /* Hide scrollbar */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.matching::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.matching-container {
  max-width: 600px;
  width: 100%;
  text-align: center;
}

.scanning-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3rem;
}

.scanning-text {
  font-size: 1.2rem;
  font-weight: 100;
  opacity: 0.8;
  color: var(--grave-gray);
  animation: pulse-text 2s ease-in-out infinite;
}

@keyframes pulse-text {
  0%, 100% { opacity: 0.5; }
  50% { opacity: 1; }
}

.waveform {
  width: 300px;
  height: 80px;
}

.wave-path {
  animation: wave-move 2s ease-in-out infinite;
}

@keyframes wave-move {
  0%, 100% {
    d: path("M 0 30 Q 25 10, 50 30 T 100 30 T 150 30 T 200 30");
  }
  25% {
    d: path("M 0 30 Q 25 50, 50 30 T 100 30 T 150 30 T 200 30");
  }
  50% {
    d: path("M 0 30 Q 25 10, 50 30 T 100 30 T 150 30 T 200 30");
  }
  75% {
    d: path("M 0 30 Q 25 30, 50 50 T 100 10 T 150 50 T 200 30");
  }
}

.match-found {
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.match-info {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  animation: slideUp 800ms ease-out;
}

.match-title {
  font-size: 1rem;
  font-weight: 200;
  opacity: 0.7;
  letter-spacing: 0.3em;
  color: var(--grave-gray);
}

.match-id {
  font-size: 2rem;
  font-weight: 100;
  color: var(--sunset-amber);
  letter-spacing: 0.2em;
}

.match-subtitle {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.5;
  letter-spacing: 0.2em;
  color: var(--grave-gray);
}

.match-description {
  font-size: 1rem;
  font-weight: 200;
  line-height: 2.2;
  opacity: 0.85;
  margin-top: 1rem;
  color: var(--grave-gray);
}

.breathing-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  margin-top: 2rem;
}

.breathing-prompt {
  font-size: 0.95rem;
  font-weight: 200;
  opacity: 0.8;
  color: var(--grave-gray);
}

.breathe-btn {
  background: none;
  border: 2px solid var(--grave-gray);
  color: var(--grave-gray);
  font-family: inherit;
  font-size: 1rem;
  font-weight: 200;
  letter-spacing: 0.2em;
  text-transform: lowercase;
  padding: 1.5rem 3rem;
  cursor: pointer;
  transition: all var(--transition-slow);
  opacity: 0.8;
  display: flex;
  align-items: center;
  gap: 1rem;
  position: relative;
}

.breathe-circle {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--grave-gray);
  transition: all var(--transition-slow);
}

.breathe-btn.active {
  border-color: var(--sunset-amber);
  color: var(--sunset-amber);
  opacity: 1;
}

.breathe-btn.active .breathe-circle {
  background: var(--sunset-amber);
  animation: breathe-pulse 3s ease-in-out infinite;
}

.breathe-btn:hover {
  opacity: 1;
  box-shadow: 0 0 30px rgba(26, 26, 27, 0.2);
}

@keyframes breathe-pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.5);
    opacity: 0.6;
  }
}

.continue-btn {
  background: none;
  border: 1px solid var(--sunset-amber);
  color: var(--sunset-amber);
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 200;
  letter-spacing: 0.25em;
  text-transform: lowercase;
  padding: 1rem 2.5rem;
  cursor: pointer;
  margin-top: 2rem;
  transition: all var(--transition-slow);
  opacity: 0.7;
}

.continue-btn:hover {
  opacity: 1;
  background: rgba(255, 157, 0, 0.1);
  box-shadow: 0 0 30px rgba(255, 157, 0, 0.3);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity var(--transition-slow);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-up-enter-active {
  animation: slideUp 1s ease-out;
}

.slide-up-leave-active {
  transition: opacity var(--transition-slow);
}

.slide-up-enter-from,
.slide-up-leave-to {
  opacity: 0;
}
</style>
