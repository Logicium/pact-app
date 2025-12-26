<script setup lang="ts">
import { ref, onMounted, computed, onUnmounted } from 'vue'

const props = defineProps<{
  answers: {
    nickname: string
    soundOfWorld: string
    geometry: string
    coreAbsence: string
    surprise: number
    physicality: string
    unfinishedBusiness: string
    ghostLetter: string
    theResolve: string
  }
}>()

const emit = defineEmits(['initiate'])

const dissolutionProgress = ref(0)
const isPulseTouching = ref(false)
const pulseInterval = ref<number | null>(null)
const pulseCount = ref(0)
const isFadingOut = ref(false)

const partnerNickname = ref('willow')

// Countdown timer (72 hours in seconds)
const COUNTDOWN_DURATION = 72 * 60 * 60
const countdownSeconds = ref(COUNTDOWN_DURATION)
const countdownInterval = ref<number | null>(null)

const countdownDisplay = computed(() => {
  const hours = Math.floor(countdownSeconds.value / 3600)
  const minutes = Math.floor((countdownSeconds.value % 3600) / 60)
  const seconds = countdownSeconds.value % 60
  return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

// Determine transition method based on geometry answer
const transitionMethod = computed(() => {
  switch (props.answers.geometry) {
    case 'drop':
    case 'horizon':
      return {
        type: 'last-breath',
        name: 'the last breath',
        description: 'helium-infused transition mask',
        details: 'soft embrace. no panic. just the slow fade.'
      }
    default: // circle
      return {
        type: 'provision',
        name: 'the provision',
        description: 'pharmaceutical grade lullaby',
        details: 'tasteless. painless. like falling into sleep.'
      }
  }
})

// Determine transition location based on geometry answer
const transitionLocation = computed(() => {
  switch (props.answers.geometry) {
    case 'drop':
      return {
        name: 'highrise observation deck',
        description: 'visibility is high tonight. a clear sky for the end.',
        coordinates: 'north tower, floor 47',
        temperature: '52°f',
        windSpeed: '4 mph'
      }
    case 'horizon':
      return {
        name: 'coastal overlook',
        description: 'the horizon line is unbroken. the sea is waiting.',
        coordinates: 'point epsilon, mile marker 23',
        temperature: '48°f',
        windSpeed: '6 mph'
      }
    default: // circle
      return {
        name: 'memorial bridge',
        description: 'visibility is high tonight. a clear sky for the end.',
        coordinates: 'west span, center arch',
        temperature: '51°f',
        windSpeed: '3 mph'
      }
  }
})

onMounted(() => {
  // Slowly increase dissolution progress
  const interval = setInterval(() => {
    if (dissolutionProgress.value < 100) {
      dissolutionProgress.value += 0.5
    } else {
      clearInterval(interval)
    }
  }, 200)

  // Start countdown if user is hesitating
  if (props.answers.theResolve === 'hesitation') {
    countdownInterval.value = window.setInterval(() => {
      if (countdownSeconds.value > 0) {
        countdownSeconds.value--
        
        // Haptic feedback every 60 seconds (if supported)
        if (countdownSeconds.value % 60 === 0 && 'vibrate' in navigator) {
          navigator.vibrate(100)
        }
      } else {
        if (countdownInterval.value) {
          clearInterval(countdownInterval.value)
        }
      }
    }, 1000)
  }
})

onUnmounted(() => {
  if (countdownInterval.value) {
    clearInterval(countdownInterval.value)
  }
  if (pulseInterval.value) {
    clearInterval(pulseInterval.value)
  }
})

const startPulseTouch = () => {
  isPulseTouching.value = true
  pulseCount.value = 0
  
  pulseInterval.value = window.setInterval(() => {
    pulseCount.value++
  }, 1200) // Heartbeat rhythm
}

const endPulseTouch = () => {
  isPulseTouching.value = false
  if (pulseInterval.value) {
    clearInterval(pulseInterval.value)
  }
}

const printGhostLetter = () => {
  const printWindow = window.open('', '_blank')
  if (printWindow) {
    printWindow.document.write(`
      <!DOCTYPE html>
      <html>
        <head>
          <title>Ghost Letter</title>
          <style>
            @page {
              margin: 2cm;
              size: A4;
            }
            body {
              font-family: 'Courier New', monospace;
              color: #1a1a1b;
              background: #e8e2d9;
              padding: 3cm;
              line-height: 2.2;
              font-size: 12pt;
            }
            .header {
              text-align: center;
              margin-bottom: 3cm;
              opacity: 0.4;
              font-size: 9pt;
              letter-spacing: 0.3em;
              text-transform: uppercase;
            }
            .content {
              white-space: pre-wrap;
              opacity: 0.85;
            }
            .footer {
              margin-top: 4cm;
              text-align: center;
              opacity: 0.3;
              font-size: 8pt;
              letter-spacing: 0.2em;
            }
          </style>
        </head>
        <body>
          <div class="header">the linen box</div>
          <div class="content">${props.answers.ghostLetter}</div>
          <div class="footer">sealed · to be released upon cessation</div>
        </body>
      </html>
    `)
    printWindow.document.close()
    setTimeout(() => {
      printWindow.print()
    }, 250)
  }
}

const initiateTransition = () => {
  isFadingOut.value = true
  
  // Wait for fade-out to complete before emitting
  setTimeout(() => {
    emit('initiate')
  }, 1500)
}
</script>

<template>
  <div class="dashboard" :class="{ 'fading-out': isFadingOut }">
    <div class="dashboard-container">
      <header class="dashboard-header">
        <h1 class="app-title">pact</h1>
        <p class="user-id">{{ answers.nickname }} × {{ partnerNickname }}</p>
        <p class="user-subtitle">user 0922 × user 771</p>
      </header>

      <div class="dashboard-grid">
        <!-- The Geometry of Rest (only for drop - falling from location) -->
        <section v-if="props.answers.geometry === 'drop'" class="card geometry-card">
          <h2 class="card-title">the geometry of rest</h2>
          <div class="map-container">
            <svg class="transition-map" viewBox="0 0 300 200" xmlns="http://www.w3.org/2000/svg">
              <!-- Highrise/Location visualization -->
              <rect x="125" y="50" width="50" height="120" fill="none" stroke="var(--grave-gray)" stroke-width="2" opacity="0.3"/>
              <line x1="150" y1="50" x2="150" y2="170" stroke="var(--sunset-amber)" stroke-width="2" opacity="0.6" stroke-dasharray="5,5"/>
              
              <!-- Pulsing location marker -->
              <circle class="location-marker" cx="150" cy="100" r="5" fill="var(--sunset-amber)"/>
            </svg>
          </div>
          <div class="location-info">
            <p class="location-name">highrise observation deck</p>
            <p class="location-description">visibility is high tonight. a clear sky for the end.</p>
            <div class="location-stats">
              <span class="stat-item">52°f</span>
              <span class="stat-separator">·</span>
              <span class="stat-item">4 mph wind</span>
            </div>
            <p class="location-coordinates">north tower, floor 47</p>
          </div>
        </section>

        <!-- The Last Breath (only for horizon - mask/suffocation, NO map) -->
        <section v-if="props.answers.geometry === 'horizon'" class="card lastbreath-card">
          <h2 class="card-title">the last breath</h2>
          <div class="provision-status">
            <div class="provision-icon">
              <div class="mask-icon"></div>
            </div>
            <p class="provision-type">helium-infused transition mask</p>
            <p class="provision-distance">2.4 miles away</p>
            <p class="provision-instruction">soft embrace. no panic. just the slow fade.</p>
          </div>
          <div class="provision-contents">
            <p class="contents-label">contents:</p>
            <ul class="contents-list">
              <li>helium transition mask (medical grade)</li>
              <li>flow regulator (pre-calibrated)</li>
              <li>comfort pillow (memory foam)</li>
            </ul>
          </div>
        </section>

        <!-- The Provision (only for circle - circular pill) -->
        <section v-if="props.answers.geometry === 'circle'" class="card provision-card">
          <h2 class="card-title">the provision</h2>
          <div class="provision-status">
            <div class="provision-icon">
              <div class="box-icon"></div>
            </div>
            <p class="provision-type">pharmaceutical grade lullaby</p>
            <p class="provision-distance">2.4 miles away</p>
            <p class="provision-instruction">tasteless. painless. like falling into sleep.</p>
          </div>
          <div class="provision-contents">
            <p class="contents-label">contents:</p>
            <ul class="contents-list">
              <li>the lullaby (glass vial)</li>
              <li>scented candle (lavender & cedar)</li>
              <li>final letter template</li>
            </ul>
          </div>
        </section>

        <!-- The Eraser -->
        <section class="card eraser-card">
          <h2 class="card-title">digital footprint dissolution</h2>
          <div class="eraser-content">
            <div class="progress-bar">
              <div class="progress-fill" :style="{ width: dissolutionProgress + '%' }"></div>
            </div>
            <p class="progress-text">{{ Math.floor(dissolutionProgress) }}% complete</p>
            <ul class="dissolution-list">
              <li :class="{ completed: dissolutionProgress > 20 }">social media accounts</li>
              <li :class="{ completed: dissolutionProgress > 40 }">email archives</li>
              <li :class="{ completed: dissolutionProgress > 60 }">cloud storage</li>
              <li :class="{ completed: dissolutionProgress > 80 }">financial records</li>
              <li :class="{ completed: dissolutionProgress >= 100 }">search history</li>
            </ul>
          </div>
        </section>

        <!-- Pulse-Touch -->
        <section class="card pulse-card">
          <h2 class="card-title">pulse-touch</h2>
          <p class="pulse-description">hold to feel their heartbeat</p>
          <div 
            class="pulse-area"
            :class="{ active: isPulseTouching }"
            @mousedown="startPulseTouch"
            @mouseup="endPulseTouch"
            @mouseleave="endPulseTouch"
            @touchstart.prevent="startPulseTouch"
            @touchend.prevent="endPulseTouch"
          >
            <div class="pulse-circle" :class="{ beating: isPulseTouching }"></div>
            <p v-if="isPulseTouching" class="pulse-status">synchronized</p>
            <p v-else class="pulse-status">touch to connect</p>
          </div>
        </section>

        <!-- The Vigil -->
        <section class="card vigil-card full-width">
          <h2 class="card-title">the vigil</h2>
          <p class="vigil-description">a shared space. watch together. breathe together.</p>
          <div class="vigil-ambient">
            <div class="ambient-scene">
              <!-- Falling snow animation -->
              <div class="snowflake" v-for="i in 20" :key="i" :style="{ 
                left: Math.random() * 100 + '%',
                animationDelay: Math.random() * 10 + 's',
                animationDuration: (10 + Math.random() * 10) + 's'
              }"></div>
            </div>
            <p class="vigil-status">{{ partnerNickname }} is watching with you</p>
          </div>
        </section>

        <!-- The Countdown (only if hesitation) -->
        <section v-if="props.answers.theResolve === 'hesitation'" class="card countdown-card full-width">
          <h2 class="card-title">the waiting room</h2>
          <p class="countdown-instruction">
            your echo is waiting for someone who is sure.<br />
            we can give you time to look back one last time.<br />
            but the door will not stay open forever.
          </p>
          <div class="countdown-display">
            <span class="countdown-time">{{ countdownDisplay }}</span>
          </div>
          <p class="countdown-warning">
            your match with {{ answers.nickname }} × {{ partnerNickname }} expires in {{ countdownDisplay }}.<br />
            if you do not step through the doorway by then, you will be returned to the world.
          </p>
        </section>

        <!-- The Ghost Letter (only if letter was written) -->
        <section v-if="props.answers.ghostLetter" class="card ghost-letter-card full-width">
          <h2 class="card-title">the linen box</h2>
          <p class="ghost-letter-description">
            your final words are sealed and safe.<br />
            when your pulse-touch ceases, they will be released.
          </p>
          <button class="print-letter-btn" @click="printGhostLetter">
            print as keepsake
          </button>
          <p class="ghost-letter-note">perfectly formatted. minimalist. like a high-end death certificate.</p>
        </section>
      </div>

      <footer class="dashboard-footer">
        <button class="initiate-btn" @click="initiateTransition">
          initiate transition
        </button>
        <p class="footer-text">our journey. our peace. the others don't understand.</p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.dashboard {
  width: 100vw;
  min-height: 100vh;
  background: var(--linen-white);
  padding: 2rem;
  overflow-y: auto;
  opacity: 1;
  transition: opacity 1.5s ease-out;
  /* Hide scrollbar */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.dashboard.fading-out {
  opacity: 0;
}

.dashboard::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.dashboard-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.dashboard-header {
  text-align: center;
  padding: 2rem 0;
  border-bottom: 1px solid rgba(26, 26, 27, 0.1);
}

.app-title {
  font-size: 2.5rem;
  font-weight: 100;
  letter-spacing: 0.5em;
  margin-bottom: 0.5rem;
  color: var(--grave-gray);
}

.user-id {
  font-size: 1.1rem;
  font-weight: 200;
  opacity: 0.8;
  letter-spacing: 0.3em;
  color: var(--sunset-amber);
  margin-bottom: 0.3rem;
}

.user-subtitle {
  font-size: 0.75rem;
  font-weight: 200;
  opacity: 0.4;
  letter-spacing: 0.25em;
  color: var(--grave-gray);
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.card {
  background: rgba(26, 26, 27, 0.02);
  border: 1px solid rgba(26, 26, 27, 0.1);
  padding: 2rem;
  transition: all var(--transition-slow);
  animation: fadeInSlideUp 1s ease-out backwards;
}

.geometry-card {
  animation-delay: 0.1s;
}

.lastbreath-card {
  animation-delay: 0.1s;
}

.provision-card {
  animation-delay: 0.2s;
}

.eraser-card {
  animation-delay: 0.3s;
}

.pulse-card {
  animation-delay: 0.4s;
}

.vigil-card {
  animation-delay: 0.5s;
}

@keyframes fadeInSlideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.card:hover {
  background: rgba(26, 26, 27, 0.04);
  border-color: rgba(26, 26, 27, 0.2);
}

.card-title {
  font-size: 1rem;
  font-weight: 200;
  letter-spacing: 0.3em;
  margin-bottom: 2rem;
  opacity: 0.8;
  color: var(--grave-gray);
}

.full-width {
  grid-column: 1 / -1;
}

/* Geometry of Rest */
.geometry-card {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.map-container {
  width: 100%;
  aspect-ratio: 3/2;
}

.transition-map {
  width: 100%;
  height: 100%;
}

.location-marker {
  animation: pulse 2s ease-in-out infinite;
}

.location-info {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.location-name {
  font-size: 1.1rem;
  font-weight: 200;
  color: var(--sunset-amber);
}

.location-description {
  font-size: 0.9rem;
  font-weight: 200;
  line-height: 1.8;
  opacity: 0.8;
}

.location-stats {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin: 1rem 0;
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.7;
  color: var(--grave-gray);
}

.stat-item {
  letter-spacing: 0.1em;
}

.stat-separator {
  opacity: 0.4;
}

.location-coordinates {
  font-size: 0.75rem;
  font-weight: 200;
  opacity: 0.5;
  letter-spacing: 0.2em;
}

/* The Provision */
.provision-status {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.provision-icon {
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.box-icon {
  width: 50px;
  height: 50px;
  border: 2px solid var(--sunset-amber);
  animation: breathe 4s ease-in-out infinite;
}

.mask-icon {
  width: 60px;
  height: 40px;
  border: 2px solid var(--sunset-amber);
  border-radius: 50% 50% 40% 40%;
  position: relative;
  animation: breathe 4s ease-in-out infinite;
}

.mask-icon::before {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  width: 2px;
  height: 12px;
  background: var(--sunset-amber);
}

.provision-type {
  font-size: 1rem;
  font-weight: 200;
  color: var(--sunset-amber);
}

.provision-distance {
  font-size: 1.2rem;
  font-weight: 200;
  opacity: 0.9;
  color: var(--grave-gray);
}

.provision-instruction {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.7;
  font-style: italic;
  color: var(--grave-gray);
}

.provision-contents {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.contents-label {
  font-size: 0.8rem;
  font-weight: 200;
  opacity: 0.6;
  letter-spacing: 0.2em;
  color: var(--grave-gray);
}

.contents-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding-left: 1rem;
}

.contents-list li {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.7;
  position: relative;
  color: var(--grave-gray);
}

.contents-list li::before {
  content: '·';
  position: absolute;
  left: -1rem;
  color: var(--sunset-amber);
}

/* The Eraser */
.eraser-content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.progress-bar {
  width: 100%;
  height: 4px;
  background: rgba(26, 26, 27, 0.1);
  position: relative;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: var(--sunset-amber);
  transition: width 0.5s ease-out;
  box-shadow: 0 0 10px rgba(255, 157, 0, 0.5);
}

.progress-text {
  font-size: 1.5rem;
  font-weight: 100;
  color: var(--sunset-amber);
  text-align: center;
}

.dissolution-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.dissolution-list li {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.4;
  transition: all var(--transition-slow);
  padding-left: 1.5rem;
  position: relative;
}

.dissolution-list li::before {
  content: '○';
  position: absolute;
  left: 0;
  transition: all var(--transition-slow);
}

.dissolution-list li.completed {
  opacity: 0.8;
  text-decoration: line-through;
}

.dissolution-list li.completed::before {
  content: '●';
  color: var(--sunset-amber);
}

/* Pulse-Touch */
.pulse-description {
  font-size: 0.9rem;
  font-weight: 200;
  opacity: 0.7;
  text-align: center;
  margin-bottom: 1rem;
  color: var(--grave-gray);
}

.pulse-area {
  width: 200px;
  height: 200px;
  margin: 0 auto;
  border: 2px solid rgba(26, 26, 27, 0.2);
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all var(--transition-slow);
  gap: 1rem;
}

.pulse-area:hover {
  border-color: var(--sunset-amber);
  box-shadow: 0 0 30px rgba(255, 157, 0, 0.2);
}

.pulse-area.active {
  border-color: var(--sunset-amber);
  background: rgba(255, 157, 0, 0.05);
}

.pulse-circle {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: var(--grave-gray);
  opacity: 0.3;
  transition: all 0.3s;
}

.pulse-circle.beating {
  background: var(--sunset-amber);
  opacity: 0.8;
  animation: heartbeat 1.2s ease-in-out infinite;
}

@keyframes heartbeat {
  0%, 100% {
    transform: scale(1);
  }
  14% {
    transform: scale(1.3);
  }
  28% {
    transform: scale(1);
  }
  42% {
    transform: scale(1.3);
  }
  56% {
    transform: scale(1);
  }
}

.pulse-status {
  font-size: 0.8rem;
  font-weight: 200;
  opacity: 0.6;
  letter-spacing: 0.2em;
  color: var(--grave-gray);
}

/* The Vigil */
.vigil-description {
  font-size: 0.95rem;
  font-weight: 200;
  opacity: 0.7;
  text-align: center;
  margin-bottom: 2rem;
  color: var(--grave-gray);
}

.vigil-ambient {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.ambient-scene {
  width: 100%;
  height: 300px;
  background: linear-gradient(to bottom, rgba(26, 26, 27, 0.03), rgba(26, 26, 27, 0.06));
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(26, 26, 27, 0.1);
}

.snowflake {
  position: absolute;
  top: -10px;
  width: 3px;
  height: 3px;
  background: var(--grave-gray);
  border-radius: 50%;
  opacity: 0.6;
  animation: snowfall 10s linear infinite;
}

@keyframes snowfall {
  0% {
    transform: translateY(0);
    opacity: 0;
  }
  10% {
    opacity: 0.6;
  }
  90% {
    opacity: 0.6;
  }
  100% {
    transform: translateY(300px);
    opacity: 0;
  }
}

.vigil-status {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.6;
  text-align: center;
  letter-spacing: 0.2em;
  color: var(--grave-gray);
}

/* Footer */
.dashboard-footer {
  text-align: center;
  padding: 3rem 0 2rem;
  border-top: 1px solid rgba(26, 26, 27, 0.1);
  display: flex;
  flex-direction: column;
  gap: 2rem;
  align-items: center;
}

.initiate-btn {
  background: var(--grave-gray);
  border: none;
  color: var(--linen-white);
  font-family: inherit;
  font-size: 1.1rem;
  font-weight: 200;
  letter-spacing: 0.3em;
  text-transform: lowercase;
  padding: 1.75rem 4rem;
  cursor: pointer;
  transition: all var(--transition-slow);
  position: relative;
  overflow: hidden;
}

.initiate-btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: var(--sunset-amber);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.8s, height 0.8s;
  z-index: 0;
  opacity: 0.3;
}

.initiate-btn:hover::before {
  width: 400px;
  height: 400px;
}

.initiate-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 40px rgba(26, 26, 27, 0.3);
}

.footer-text {
  font-size: 0.9rem;
  font-weight: 200;
  opacity: 0.5;
  letter-spacing: 0.2em;
  font-style: italic;
  color: var(--grave-gray);
}

/* Countdown Card */
.countdown-card {
  background: rgba(26, 26, 27, 0.03);
  border: 2px solid rgba(255, 157, 0, 0.3);
}

.countdown-instruction {
  font-size: 0.95rem;
  line-height: 2.2;
  text-align: center;
  opacity: 0.8;
  margin-bottom: 3rem;
  font-weight: 200;
  color: var(--grave-gray);
}

.countdown-display {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}

.countdown-time {
  font-size: 4rem;
  font-weight: 100;
  letter-spacing: 0.15em;
  color: var(--sunset-amber);
  font-variant-numeric: tabular-nums;
  text-shadow: 0 0 20px rgba(255, 157, 0, 0.3);
}

.countdown-warning {
  font-size: 0.85rem;
  line-height: 2;
  text-align: center;
  opacity: 0.6;
  margin-top: 2rem;
  font-weight: 200;
  color: var(--grave-gray);
}

/* Ghost Letter Card */
.ghost-letter-card {
  background: rgba(26, 26, 27, 0.02);
  text-align: center;
}

.ghost-letter-description {
  font-size: 0.95rem;
  line-height: 2.2;
  opacity: 0.8;
  margin-bottom: 2rem;
  font-weight: 200;
  color: var(--grave-gray);
}

.print-letter-btn {
  background: none;
  border: 1px solid var(--grave-gray);
  color: var(--grave-gray);
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 200;
  letter-spacing: 0.2em;
  text-transform: lowercase;
  padding: 1rem 3rem;
  cursor: pointer;
  transition: all var(--transition-slow);
  opacity: 0.7;
  margin: 1.5rem 0;
}

.print-letter-btn:hover {
  opacity: 1;
  border-color: var(--sunset-amber);
  color: var(--sunset-amber);
  box-shadow: 0 0 20px rgba(255, 157, 0, 0.2);
}

.ghost-letter-note {
  font-size: 0.75rem;
  opacity: 0.4;
  font-style: italic;
  margin-top: 1rem;
  font-weight: 200;
  color: var(--grave-gray);
}

@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .countdown-time {
    font-size: 2.5rem;
  }
}
</style>
