<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'

const props = defineProps<{
  answers: {
    soundOfWorld: string
    geometry: string
    surprise: number
    physicality: string
  }
}>()

const dissolutionProgress = ref(0)
const isPulseTouching = ref(false)
const pulseInterval = ref<number | null>(null)
const pulseCount = ref(0)

// Determine transition location based on geometry answer
const transitionLocation = computed(() => {
  switch (props.answers.geometry) {
    case 'drop':
      return {
        name: 'highrise observation deck',
        description: 'visibility is high tonight. a clear sky for the end.',
        coordinates: 'north tower, floor 47'
      }
    case 'horizon':
      return {
        name: 'coastal overlook',
        description: 'the horizon line is unbroken. the sea is waiting.',
        coordinates: 'point epsilon, mile marker 23'
      }
    default: // circle
      return {
        name: 'memorial bridge',
        description: 'visibility is high tonight. a clear sky for the end.',
        coordinates: 'west span, center arch'
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
</script>

<template>
  <div class="dashboard">
    <div class="dashboard-container">
      <header class="dashboard-header">
        <h1 class="app-title">pact</h1>
        <p class="user-id">user 0922 × user 771</p>
      </header>

      <div class="dashboard-grid">
        <!-- The Geometry of Rest -->
        <section class="card geometry-card">
          <h2 class="card-title">the geometry of rest</h2>
          <div class="map-container">
            <svg class="transition-map" viewBox="0 0 300 200" xmlns="http://www.w3.org/2000/svg">
              <!-- Bridge/Location visualization -->
              <line v-if="props.answers.geometry === 'circle'" x1="50" y1="100" x2="250" y2="100" stroke="var(--linen-white)" stroke-width="2" opacity="0.3"/>
              <circle v-if="props.answers.geometry === 'circle'" cx="150" cy="100" r="40" fill="none" stroke="var(--sunset-amber)" stroke-width="2" opacity="0.6"/>
              
              <rect v-if="props.answers.geometry === 'drop'" x="125" y="50" width="50" height="120" fill="none" stroke="var(--linen-white)" stroke-width="2" opacity="0.3"/>
              <line v-if="props.answers.geometry === 'drop'" x1="150" y1="50" x2="150" y2="170" stroke="var(--sunset-amber)" stroke-width="2" opacity="0.6" stroke-dasharray="5,5"/>
              
              <line v-if="props.answers.geometry === 'horizon'" x1="0" y1="100" x2="300" y2="100" stroke="var(--linen-white)" stroke-width="2" opacity="0.3"/>
              <line v-if="props.answers.geometry === 'horizon'" x1="0" y1="100" x2="300" y2="100" stroke="var(--sunset-amber)" stroke-width="3" opacity="0.6"/>
              
              <!-- Pulsing location marker -->
              <circle class="location-marker" cx="150" cy="100" r="5" fill="var(--sunset-amber)"/>
            </svg>
          </div>
          <div class="location-info">
            <p class="location-name">{{ transitionLocation.name }}</p>
            <p class="location-description">{{ transitionLocation.description }}</p>
            <p class="location-coordinates">{{ transitionLocation.coordinates }}</p>
          </div>
        </section>

        <!-- The Provision -->
        <section class="card provision-card">
          <h2 class="card-title">the provision</h2>
          <div class="provision-status">
            <div class="provision-icon">
              <div class="box-icon"></div>
            </div>
            <p class="provision-type">linen box (grade-a)</p>
            <p class="provision-distance">2.4 miles away</p>
            <p class="provision-instruction">prepare the candle.</p>
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
            <p class="vigil-status">user 771 is watching with you</p>
          </div>
        </section>
      </div>

      <footer class="dashboard-footer">
        <p class="footer-text">our journey. our peace. the others don't understand.</p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.dashboard {
  width: 100vw;
  min-height: 100vh;
  background: var(--grave-gray);
  padding: 2rem;
  overflow-y: auto;
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
  border-bottom: 1px solid rgba(232, 226, 217, 0.1);
}

.app-title {
  font-size: 2.5rem;
  font-weight: 100;
  letter-spacing: 0.5em;
  margin-bottom: 0.5rem;
}

.user-id {
  font-size: 0.9rem;
  font-weight: 200;
  opacity: 0.6;
  letter-spacing: 0.3em;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.card {
  background: rgba(232, 226, 217, 0.02);
  border: 1px solid rgba(232, 226, 217, 0.1);
  padding: 2rem;
  transition: all var(--transition-slow);
  animation: slideUp 1s ease-out;
}

.card:hover {
  background: rgba(232, 226, 217, 0.04);
  border-color: rgba(232, 226, 217, 0.2);
}

.card-title {
  font-size: 1rem;
  font-weight: 200;
  letter-spacing: 0.3em;
  margin-bottom: 2rem;
  opacity: 0.8;
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

.provision-type {
  font-size: 1rem;
  font-weight: 200;
  color: var(--sunset-amber);
}

.provision-distance {
  font-size: 1.2rem;
  font-weight: 200;
  opacity: 0.9;
}

.provision-instruction {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.7;
  font-style: italic;
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
  background: rgba(232, 226, 217, 0.1);
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
}

.pulse-area {
  width: 200px;
  height: 200px;
  margin: 0 auto;
  border: 2px solid rgba(232, 226, 217, 0.2);
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
  background: var(--linen-white);
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
}

/* The Vigil */
.vigil-description {
  font-size: 0.95rem;
  font-weight: 200;
  opacity: 0.7;
  text-align: center;
  margin-bottom: 2rem;
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
  background: linear-gradient(to bottom, rgba(26, 26, 27, 0.8), rgba(26, 26, 27, 0.95));
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(232, 226, 217, 0.1);
}

.snowflake {
  position: absolute;
  top: -10px;
  width: 3px;
  height: 3px;
  background: var(--linen-white);
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
}

/* Footer */
.dashboard-footer {
  text-align: center;
  padding: 3rem 0 2rem;
  border-top: 1px solid rgba(232, 226, 217, 0.1);
}

.footer-text {
  font-size: 0.9rem;
  font-weight: 200;
  opacity: 0.5;
  letter-spacing: 0.2em;
  font-style: italic;
}

@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
}
</style>
