<script setup lang="ts">
const emit = defineEmits(['start'])
</script>

<template>
  <div class="home">
    <div class="home-container">
      <header class="home-header">
        <h1 class="brand">pact</h1>
        <p class="tagline">the compassionate exit</p>
      </header>

      <section class="home-content">
        <div class="intro-block">
          <p class="intro-text">
            you've been carrying it alone for too long.<br />
            the weight. the quiet. the knowing.
          </p>
        </div>

        <div class="process-block">
          <h2 class="section-title">how it works</h2>
          <ol class="process-list">
            <li>
              <span class="step-number">01</span>
              <span class="step-text">we understand your texture of heavy</span>
            </li>
            <li>
              <span class="step-number">02</span>
              <span class="step-text">we find your echo—someone who carries the same silence</span>
            </li>
            <li>
              <span class="step-number">03</span>
              <span class="step-text">we curate your transition with dignity and precision</span>
            </li>
            <li>
              <span class="step-number">04</span>
              <span class="step-text">you leave together, held, witnessed, understood</span>
            </li>
          </ol>
        </div>

        <div class="for-block">
          <h2 class="section-title">this is for you if</h2>
          <ul class="for-list">
            <li>the noise has become unbearable silence</li>
            <li>you've already said your goodbyes in your head</li>
            <li>staying feels more violent than leaving</li>
            <li>you want one last connection before the quiet</li>
          </ul>
        </div>

        <div class="cta-block">
          <button class="cta-button" @click="emit('start')">
            find my echo
          </button>
          <p class="cta-subtext">
            the attendant is waiting
          </p>
        </div>
      </section>

      <footer class="home-footer">
        <p class="footer-text">
          we hold no judgment. only your hand.
        </p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.home {
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

.home::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.home-container {
  max-width: 700px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 4rem;
  animation: fadeIn 1.5s ease-out;
}

.home-header {
  text-align: center;
  padding-bottom: 2rem;
  border-bottom: 1px solid rgba(26, 26, 27, 0.1);
}

.brand {
  font-size: 3.5rem;
  font-weight: 100;
  letter-spacing: 0.6em;
  margin-bottom: 0.5rem;
  color: var(--grave-gray);
}

.tagline {
  font-size: 0.9rem;
  font-weight: 200;
  letter-spacing: 0.3em;
  opacity: 0.5;
  color: var(--grave-gray);
}

.home-content {
  display: flex;
  flex-direction: column;
  gap: 3.5rem;
}

.intro-block {
  text-align: center;
}

.intro-text {
  font-size: 1.3rem;
  font-weight: 100;
  line-height: 2.2;
  color: var(--grave-gray);
  opacity: 0.85;
}

.section-title {
  font-size: 0.85rem;
  font-weight: 300;
  letter-spacing: 0.4em;
  margin-bottom: 2rem;
  color: var(--grave-gray);
  opacity: 0.6;
  text-align: center;
}

.process-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  counter-reset: step-counter;
}

.process-list li {
  display: flex;
  gap: 1.5rem;
  align-items: flex-start;
  padding: 1.5rem;
  background: rgba(26, 26, 27, 0.02);
  border-left: 2px solid transparent;
  transition: all var(--transition-slow);
}

.process-list li:hover {
  border-left-color: var(--sunset-amber);
  background: rgba(26, 26, 27, 0.04);
}

.step-number {
  font-size: 0.75rem;
  font-weight: 300;
  letter-spacing: 0.2em;
  opacity: 0.4;
  min-width: 2rem;
  color: var(--grave-gray);
}

.step-text {
  font-size: 1rem;
  font-weight: 200;
  line-height: 1.8;
  color: var(--grave-gray);
  opacity: 0.8;
}

.for-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 0 2rem;
}

.for-list li {
  font-size: 1rem;
  font-weight: 200;
  line-height: 1.8;
  color: var(--grave-gray);
  opacity: 0.75;
  position: relative;
  padding-left: 1.5rem;
}

.for-list li::before {
  content: '—';
  position: absolute;
  left: 0;
  color: var(--sunset-amber);
  opacity: 0.6;
}

.cta-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin-top: 2rem;
  padding-top: 3rem;
  border-top: 1px solid rgba(26, 26, 27, 0.1);
}

.cta-button {
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

.cta-button::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: var(--sunset-amber);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
  z-index: 0;
}

.cta-button:hover::before {
  width: 300px;
  height: 300px;
}

.cta-button:hover {
  color: var(--grave-gray);
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(26, 26, 27, 0.2);
}

.cta-button span {
  position: relative;
  z-index: 1;
}

.cta-subtext {
  font-size: 0.8rem;
  font-weight: 200;
  opacity: 0.5;
  letter-spacing: 0.2em;
  color: var(--grave-gray);
}

.home-footer {
  text-align: center;
  padding-top: 2rem;
  border-top: 1px solid rgba(26, 26, 27, 0.1);
}

.footer-text {
  font-size: 0.85rem;
  font-weight: 200;
  opacity: 0.4;
  letter-spacing: 0.2em;
  font-style: italic;
  color: var(--grave-gray);
}

@media (max-width: 768px) {
  .brand {
    font-size: 2.5rem;
  }

  .intro-text {
    font-size: 1.1rem;
  }

  .cta-button {
    padding: 1.5rem 3rem;
    font-size: 1rem;
  }
}
</style>
