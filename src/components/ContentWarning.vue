<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['acknowledge'])

const hasAcknowledged = ref(false)

const acknowledge = () => {
  hasAcknowledged.value = true
  setTimeout(() => {
    emit('acknowledge')
  }, 800)
}
</script>

<template>
  <div class="warning-overlay" :class="{ fading: hasAcknowledged }">
    <div class="warning-content">
      <div class="warning-symbol">⚠</div>
      
      <h1 class="warning-title">content warning</h1>
      
      <div class="warning-text">
        <p>
          this is a <strong>fictional promotional experience</strong> for a psychological horror film.
        </p>
        <p>
          it explores themes of suicide, despair, and psychological manipulation through design.
        </p>
        <p>
          the purpose is artistic critique of how technology can weaponize comfort.
        </p>
      </div>

      <div class="help-resources">
        <p class="help-title">if you need help:</p>
        <ul class="help-list">
          <li>988 - national suicide prevention lifeline (us)</li>
          <li>text HOME to 741741 - crisis text line</li>
          <li><a href="https://988lifeline.org" target="_blank" rel="noopener">988lifeline.org</a></li>
        </ul>
      </div>

      <button class="acknowledge-btn" @click="acknowledge">
        i understand - continue to experience
      </button>

      <p class="disclaimer">
        this is a work of fiction for educational and artistic purposes.
      </p>
    </div>
  </div>
</template>

<style scoped>
.warning-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: #000000;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: 2rem;
  transition: opacity var(--transition-slow);
}

.warning-overlay.fading {
  opacity: 0;
  pointer-events: none;
}

.warning-content {
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  text-align: center;
  animation: slideUp 1s ease-out;
}

.warning-symbol {
  font-size: 3rem;
  color: var(--sunset-amber);
  opacity: 0.8;
}

.warning-title {
  font-size: 2rem;
  font-weight: 100;
  letter-spacing: 0.4em;
  text-transform: lowercase;
  color: var(--linen-white);
}

.warning-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  color: var(--linen-white);
  line-height: 1.8;
}

.warning-text p {
  font-size: 1rem;
  font-weight: 200;
  opacity: 0.9;
}

.warning-text strong {
  color: var(--sunset-amber);
  font-weight: 300;
}

.help-resources {
  background: rgba(255, 157, 0, 0.05);
  border: 1px solid rgba(255, 157, 0, 0.3);
  padding: 1.5rem;
  margin-top: 1rem;
  width: 100%;
}

.help-title {
  font-size: 0.9rem;
  font-weight: 300;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--sunset-amber);
  margin-bottom: 1rem;
}

.help-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.help-list li {
  font-size: 0.9rem;
  font-weight: 200;
  color: var(--linen-white);
  opacity: 0.9;
}

.help-list a {
  color: var(--sunset-amber);
  text-decoration: none;
  transition: opacity var(--transition-slow);
}

.help-list a:hover {
  opacity: 0.7;
  text-decoration: underline;
}

.acknowledge-btn {
  background: none;
  border: 2px solid var(--sunset-amber);
  color: var(--sunset-amber);
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 200;
  letter-spacing: 0.2em;
  text-transform: lowercase;
  padding: 1.5rem 3rem;
  cursor: pointer;
  margin-top: 2rem;
  transition: all var(--transition-slow);
  opacity: 0.8;
}

.acknowledge-btn:hover {
  opacity: 1;
  background: rgba(255, 157, 0, 0.1);
  box-shadow: 0 0 30px rgba(255, 157, 0, 0.3);
}

.disclaimer {
  font-size: 0.75rem;
  font-weight: 200;
  opacity: 0.5;
  margin-top: 1rem;
  font-style: italic;
}

@media (max-width: 600px) {
  .warning-content {
    gap: 1.5rem;
  }

  .warning-title {
    font-size: 1.5rem;
  }

  .warning-text p {
    font-size: 0.9rem;
  }

  .acknowledge-btn {
    padding: 1.25rem 2rem;
    font-size: 0.8rem;
  }
}
</style>
