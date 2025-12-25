<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const emit = defineEmits<{
  complete: [answers: {
    nickname: string
    soundOfWorld: string
    geometry: string
    coreAbsence: string
    surprise: number
    physicality: string
  }]
}>()

const currentQuestion = ref(0)
const showQuestion = ref(false)

const answers = ref({
  nickname: '',
  soundOfWorld: '',
  geometry: '',
  coreAbsence: '',
  surprise: 50,
  physicality: ''
})

const questions = [
  {
    id: 'intro',
    type: 'intro',
    text: 'you\'ve been carrying it for a long time, haven\'t you?<br />the weight.<br /><br />you don\'t have to explain it here.<br />we already know the shape of your silence.<br /><br />before we find your echo—the person who will hold your hand through the doorway—we need to understand the texture of your \'heavy\'.'
  },
  {
    id: 'nickname',
    type: 'input',
    text: 'what should your echo call you?',
    placeholder: 'a name, a memory, anything soft'
  },
  {
    id: 'soundOfWorld',
    type: 'choice',
    text: 'the sound of the world',
    options: [
      { value: 'static', label: 'an unending static' },
      { value: 'ringing', label: 'a high-pitched ringing in an empty room' },
      { value: 'muffled', label: 'a heavy, underwater muffled thud' }
    ]
  },
  {
    id: 'geometry',
    type: 'choice',
    text: 'the geometry of the end',
    options: [
      { value: 'horizon', label: 'a long, straight line toward a horizon' },
      { value: 'circle', label: 'a perfect, closing circle' },
      { value: 'drop', label: 'a sudden, vertical drop' }
    ]
  },
  {
    id: 'coreAbsence',
    type: 'choice',
    text: 'the core of the absence',
    options: [
      { value: 'loneliness', label: 'the echo of an empty room that used to hold someone' },
      { value: 'burden', label: 'the arithmetic of survival that no longer adds up' },
      { value: 'erosion', label: 'the slow disappearing of the person you once were' },
      { value: 'exhaustion', label: 'the weight of pretending the sun still feels warm' }
    ]
  },
  {
    id: 'surprise',
    type: 'slider',
    text: 'when you are gone, who will be \'surprised\'?',
    min: 'no one',
    max: 'everyone'
  },
  {
    id: 'physicality',
    type: 'choice',
    text: 'where do you feel the \'heavy\' most?',
    options: [
      { value: 'chest', label: 'the center of the chest' },
      { value: 'neck', label: 'the back of the neck' },
      { value: 'feet', label: 'the soles of the feet' }
    ]
  }
]

const canProceed = computed(() => {
  const q = questions[currentQuestion.value]
  if (q.type === 'intro') return true
  if (q.type === 'slider') return true
  if (q.type === 'input') return answers.value.nickname.trim().length > 0
  return answers.value[q.id as keyof typeof answers.value] !== ''
})

onMounted(() => {
  setTimeout(() => {
    showQuestion.value = true
  }, 500)
})

const selectAnswer = (questionId: string, value: string) => {
  (answers.value as any)[questionId] = value
  setTimeout(nextQuestion, 800)
}

const nextQuestion = () => {
  if (currentQuestion.value < questions.length - 1) {
    showQuestion.value = false
    setTimeout(() => {
      currentQuestion.value++
      showQuestion.value = true
    }, 800)
  } else {
    completeQuiz()
  }
}

const completeQuiz = () => {
  emit('complete', answers.value)
}
</script>

<template>
  <div class="quiz">
    <div class="quiz-container">
      <transition name="slide" mode="out-in">
        <div v-if="showQuestion" :key="currentQuestion" class="question-card">
          <!-- Intro Screen -->
          <div v-if="questions[currentQuestion].type === 'intro'" class="intro-screen">
            <p class="intro-text" v-html="questions[currentQuestion].text"></p>
            <button class="continue-btn" @click="nextQuestion">
              continue
            </button>
          </div>

          <!-- Choice Questions -->
          <div v-else-if="questions[currentQuestion].type === 'choice'" class="choice-screen">
            <h2 class="question-title">{{ questions[currentQuestion].text }}</h2>
            <div class="options">
              <button
                v-for="option in questions[currentQuestion].options"
                :key="option.value"
                class="option-btn"
                :class="{ selected: answers[questions[currentQuestion].id as keyof typeof answers] === option.value }"
                @click="selectAnswer(questions[currentQuestion].id, option.value)"
              >
                {{ option.label }}
              </button>
            </div>
          </div>

          <!-- Input Question -->
          <div v-else-if="questions[currentQuestion].type === 'input'" class="input-screen">
            <h2 class="question-title">{{ questions[currentQuestion].text }}</h2>
            <input 
              type="text"
              v-model="answers.nickname"
              :placeholder="questions[currentQuestion].placeholder"
              class="nickname-input"
              @keyup.enter="canProceed && nextQuestion()"
              autofocus
            />
            <button 
              class="continue-btn" 
              :class="{ disabled: !canProceed }"
              :disabled="!canProceed"
              @click="nextQuestion"
            >
              continue
            </button>
          </div>

          <!-- Slider Question -->
          <div v-else-if="questions[currentQuestion].type === 'slider'" class="slider-screen">
            <h2 class="question-title">{{ questions[currentQuestion].text }}</h2>
            <div class="slider-container">
              <span class="slider-label">{{ questions[currentQuestion].min }}</span>
              <input
                type="range"
                min="0"
                max="100"
                v-model="answers.surprise"
                class="slider"
              />
              <span class="slider-label">{{ questions[currentQuestion].max }}</span>
            </div>
            <button class="continue-btn" @click="nextQuestion">
              continue
            </button>
          </div>
        </div>
      </transition>

      <div class="progress-indicator">
        <div 
          v-for="(q, idx) in questions.filter(q => q.type !== 'intro')" 
          :key="idx" 
          class="progress-dot"
          :class="{ active: currentQuestion > idx, current: currentQuestion === idx + 1 }"
        ></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.quiz {
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

.quiz::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.quiz-container {
  max-width: 600px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.question-card {
  animation: slideUp 800ms ease-out;
}

.intro-screen,
.choice-screen,
.input-screen,
.slider-screen {
  display: flex;
  flex-direction: column;
  gap: 3rem;
  align-items: center;
}

.intro-text {
  font-size: 1.1rem;
  line-height: 2.5;
  text-align: center;
  font-weight: 100;
  opacity: 0.9;
  color: var(--grave-gray);
}

.question-title {
  font-size: 1.3rem;
  font-weight: 200;
  text-align: center;
  opacity: 0.95;
  color: var(--grave-gray);
}

.options {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  width: 100%;
}

.option-btn {
  background: none;
  border: none;
  color: var(--grave-gray);
  font-family: inherit;
  font-size: 1rem;
  font-weight: 200;
  letter-spacing: 0.15em;
  text-transform: lowercase;
  padding: 1.5rem 2rem;
  cursor: pointer;
  position: relative;
  transition: all var(--transition-slow);
  opacity: 0.7;
}

.option-btn::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 1px;
  background: var(--grave-gray);
  transition: width var(--transition-slow);
}

.option-btn:hover::after,
.option-btn.selected::after {
  width: 80%;
}

.option-btn:hover,
.option-btn.selected {
  opacity: 1;
}

.option-btn.selected::after {
  background: var(--sunset-amber);
}

/* Input Screen Styles */
.input-screen {
  gap: 2rem;
}

.nickname-input {
  width: 100%;
  max-width: 400px;
  background: none;
  border: none;
  border-bottom: 2px solid rgba(26, 26, 27, 0.2);
  color: var(--grave-gray);
  font-family: inherit;
  font-size: 1.2rem;
  font-weight: 200;
  letter-spacing: 0.15em;
  text-transform: lowercase;
  padding: 1rem 0.5rem;
  text-align: center;
  transition: all var(--transition-slow);
  outline: none;
}

.nickname-input::placeholder {
  color: var(--grave-gray);
  opacity: 0.3;
}

.nickname-input:focus {
  border-bottom-color: var(--sunset-amber);
}

.slider-screen {
  gap: 2rem;
}

.slider-container {
  display: flex;
  align-items: center;
  gap: 2rem;
  width: 100%;
}

.slider-label {
  font-size: 0.8rem;
  font-weight: 200;
  opacity: 0.6;
  white-space: nowrap;
  color: var(--grave-gray);
}

.slider {
  flex: 1;
  -webkit-appearance: none;
  appearance: none;
  height: 2px;
  background: rgba(26, 26, 27, 0.2);
  outline: none;
  transition: all var(--transition-slow);
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  background: var(--sunset-amber);
  border-radius: 50%;
  cursor: pointer;
  transition: all var(--transition-slow);
}

.slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  background: var(--sunset-amber);
  border-radius: 50%;
  cursor: pointer;
  border: none;
  transition: all var(--transition-slow);
}

.slider:hover::-webkit-slider-thumb {
  transform: scale(1.2);
  box-shadow: 0 0 20px rgba(255, 157, 0, 0.5);
}

.slider:hover::-moz-range-thumb {
  transform: scale(1.2);
  box-shadow: 0 0 20px rgba(255, 157, 0, 0.5);
}

.continue-btn {
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
  margin-top: 2rem;
  transition: all var(--transition-slow);
  opacity: 0.7;
}

.continue-btn:hover:not(.disabled) {
  opacity: 1;
  border-color: var(--sunset-amber);
  color: var(--sunset-amber);
  box-shadow: 0 0 20px rgba(255, 157, 0, 0.2);
}

.continue-btn.disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.progress-indicator {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 2rem;
}

.progress-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(26, 26, 27, 0.2);
  transition: all var(--transition-slow);
}

.progress-dot.active {
  background: var(--grave-gray);
}

.progress-dot.current {
  background: var(--sunset-amber);
  box-shadow: 0 0 10px rgba(255, 157, 0, 0.5);
}

.slide-enter-active,
.slide-leave-active {
  transition: all var(--transition-slow);
}

.slide-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
</style>
