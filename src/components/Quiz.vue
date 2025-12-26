<script setup lang="ts">
import { ref, computed, onMounted, nextTick, watch } from 'vue'

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
const contentHeight = ref(0)
const contentRef = ref<HTMLElement | null>(null)

const answers = ref({
  nickname: '',
  soundOfWorld: '',
  geometry: '',
  coreAbsence: '',
  surprise: 50,
  physicality: '',
  unfinishedBusiness: '',
  ghostLetter: '',
  theResolve: ''
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
    id: 'surprise',
    type: 'slider',
    text: 'when you are gone, who will be \'surprised\'?',
    min: 'no one',
    max: 'everyone'
  },
  {
    id: 'unfinishedBusiness',
    type: 'choice',
    text: 'are there words still caught in your throat?<br />letters you never sent? whispers for those who will remain?',
    options: [
      { value: 'finished', label: 'i have said my goodbyes. i have already drifted away.' },
      { value: 'unsaid', label: 'there are anchors of words still holding me to the shore.' }
    ]
  },
  {
    id: 'theResolve',
    type: 'choice',
    text: 'is the decision a calm, still lake?<br />or is it still a storm?',
    options: [
      { value: 'peace', label: 'i am at peace with the quiet.' },
      { value: 'hesitation', label: 'i am still looking back at the light.' }
    ]
  }
]

const filteredQuestions = computed(() => {
  const baseQuestions = [...questions]
  
  // If user chose 'unsaid', insert the Ghost Letter after unfinishedBusiness
  if (answers.value.unfinishedBusiness === 'unsaid') {
    const businessIndex = baseQuestions.findIndex(q => q.id === 'unfinishedBusiness')
    baseQuestions.splice(businessIndex + 1, 0, {
      id: 'ghostLetter',
      type: 'textarea',
      text: 'type what you cannot say. we will keep it safe in the linen box.<br />when your pulse-touch ceases, we will release it to them.<br />it will be as if you never left a stone unturned.',
      placeholder: 'dear...'
    })
  }
  
  return baseQuestions
})

const canProceed = computed(() => {
  const q = filteredQuestions.value[currentQuestion.value]
  if (q.type === 'intro') return true
  if (q.type === 'slider') return true
  if (q.type === 'input') return answers.value.nickname.trim().length > 0
  if (q.type === 'textarea') return answers.value.ghostLetter.trim().length > 0
  return answers.value[q.id as keyof typeof answers.value] !== ''
})

onMounted(() => {
  setTimeout(() => {
    showQuestion.value = true
    nextTick(updateHeight)
  }, 500)
})

watch(showQuestion, () => {
  nextTick(updateHeight)
})

const updateHeight = () => {
  if (contentRef.value) {
    contentHeight.value = contentRef.value.offsetHeight
  }
}

const selectAnswer = (questionId: string, value: string) => {
  (answers.value as any)[questionId] = value
  setTimeout(nextQuestion, 800)
}

const nextQuestion = () => {
  if (currentQuestion.value < filteredQuestions.value.length - 1) {
    showQuestion.value = false
    setTimeout(() => {
      currentQuestion.value++
      showQuestion.value = true
    }, 600)
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
      <div class="question-wrapper" :style="{ height: contentHeight + 'px' }">
        <transition name="fade" mode="out-in">
          <div v-if="showQuestion" :key="currentQuestion" class="question-card" ref="contentRef">
            <!-- Intro Screen -->
            <div v-if="filteredQuestions[currentQuestion].type === 'intro'" class="intro-screen">
              <p class="intro-text" v-html="filteredQuestions[currentQuestion].text"></p>
              <button class="continue-btn" @click="nextQuestion">
                continue
              </button>
            </div>

            <!-- Choice Questions -->
            <div v-else-if="filteredQuestions[currentQuestion].type === 'choice'" class="choice-screen">
              <h2 class="question-title" v-html="filteredQuestions[currentQuestion].text"></h2>
              <div class="options">
                <button
                  v-for="option in filteredQuestions[currentQuestion].options"
                  :key="option.value"
                  class="option-btn"
                  :class="{ selected: answers[filteredQuestions[currentQuestion].id as keyof typeof answers] === option.value }"
                  @click="selectAnswer(filteredQuestions[currentQuestion].id, option.value)"
                >
                  {{ option.label }}
                </button>
              </div>
            </div>

            <!-- Input Question -->
            <div v-else-if="filteredQuestions[currentQuestion].type === 'input'" class="input-screen">
            <h2 class="question-title">{{ filteredQuestions[currentQuestion].text }}</h2>
            <input 
              type="text"
              v-model="answers.nickname"
              :placeholder="filteredQuestions[currentQuestion].placeholder"
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

          <!-- Textarea Question (Ghost Letter) -->
          <div v-else-if="filteredQuestions[currentQuestion].type === 'textarea'" class="textarea-screen">
            <h2 class="question-title ghost-letter-title" v-html="filteredQuestions[currentQuestion].text"></h2>
            <textarea
              v-model="answers.ghostLetter"
              :placeholder="filteredQuestions[currentQuestion].placeholder"
              class="ghost-letter-textarea"
              rows="8"
            ></textarea>
            <button 
              class="continue-btn" 
              :class="{ disabled: !canProceed }"
              :disabled="!canProceed"
              @click="nextQuestion"
            >
              seal the letter
            </button>
          </div>

          <!-- Slider Question -->
          <div v-else-if="filteredQuestions[currentQuestion].type === 'slider'" class="slider-screen">
            <h2 class="question-title">{{ filteredQuestions[currentQuestion].text }}</h2>
            <div class="slider-container">
              <span class="slider-label">{{ filteredQuestions[currentQuestion].min }}</span>
              <input
                type="range"
                min="0"
                max="100"
                v-model="answers.surprise"
                class="slider"
              />
              <span class="slider-label">{{ filteredQuestions[currentQuestion].max }}</span>
            </div>
            <button class="continue-btn" @click="nextQuestion">
              continue
            </button>
          </div>
        </div>
      </transition>
      </div>

      <div class="progress-indicator">
        <div 
          v-for="(q, idx) in filteredQuestions.filter(q => q.type !== 'intro')" 
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
  align-items: center;
  gap: 3rem;
}

.question-wrapper {
  width: 100%;
  transition: height 1.2s ease;
  position: relative;
}

.question-card {
  width: 100%;
}

.intro-screen,
.choice-screen,
.input-screen,
.slider-screen {
  display: flex;
  flex-direction: column;
  width: 100%;
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

/* Textarea Screen (Ghost Letter) */
.textarea-screen {
  gap: 2rem;
}

.ghost-letter-title {
  font-size: 1.1rem;
  line-height: 2;
  font-weight: 100;
  opacity: 0.85;
}

.ghost-letter-textarea {
  width: 100%;
  max-width: 500px;
  background: none;
  border: none;
  border-bottom: 1px solid rgba(26, 26, 27, 0.15);
  color: var(--grave-gray);
  font-family: inherit;
  font-size: 1rem;
  font-weight: 200;
  letter-spacing: 0.08em;
  line-height: 2.2;
  padding: 1.5rem 0.5rem;
  text-align: left;
  transition: all var(--transition-slow);
  outline: none;
  resize: none;
  opacity: 0.9;
}

.ghost-letter-textarea::placeholder {
  color: var(--grave-gray);
  opacity: 0.25;
  font-style: italic;
}

.ghost-letter-textarea:focus {
  border-bottom-color: var(--sunset-amber);
  opacity: 0.7;
}

/* Slider Screen */
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
  transition: transform 0.8s ease;
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

.slide-enter-active {
  transition: opacity 0.8s ease;
}

.slide-leave-active {
  transition: opacity 0.8s ease;
  position: absolute;
  width: 100%;
}

.slide-enter-from {
  opacity: 0;
}

.slide-enter-to {
  opacity: 1;
}

.slide-leave-from {
  opacity: 1;
}

.slide-leave-to {
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.6s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-active {
  transition-delay: 0.2s;
}
</style>
