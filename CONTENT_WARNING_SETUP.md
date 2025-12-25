# How to Enable the Content Warning Screen

A `ContentWarning.vue` component has been created for you. To enable it:

## Option 1: Add to App.vue (Recommended for Public Deployment)

Update your `App.vue`:

```vue
<script setup lang="ts">
import { ref } from 'vue'
import ContentWarning from './components/ContentWarning.vue'
import Opening from './components/Opening.vue'
import Quiz from './components/Quiz.vue'
import Matching from './components/Matching.vue'
import Dashboard from './components/Dashboard.vue'

type AppState = 'warning' | 'opening' | 'quiz' | 'matching' | 'dashboard'

const currentState = ref<AppState>('warning')  // Changed from 'opening'
const quizAnswers = ref({
  soundOfWorld: '',
  geometry: '',
  surprise: 50,
  physicality: ''
})

const acknowledgeWarning = () => {
  currentState.value = 'opening'
}

const advanceToQuiz = () => {
  currentState.value = 'quiz'
}

const completeQuiz = (answers: typeof quizAnswers.value) => {
  quizAnswers.value = answers
  currentState.value = 'matching'
}

const advanceToDashboard = () => {
  currentState.value = 'dashboard'
}
</script>

<template>
  <div class="app-container">
    <ContentWarning v-if="currentState === 'warning'" @acknowledge="acknowledgeWarning" />
    <Opening v-else-if="currentState === 'opening'" @begin="advanceToQuiz" />
    <Quiz v-else-if="currentState === 'quiz'" @complete="completeQuiz" />
    <Matching v-else-if="currentState === 'matching'" @matched="advanceToDashboard" :answers="quizAnswers" />
    <Dashboard v-else-if="currentState === 'dashboard'" :answers="quizAnswers" />
  </div>
</template>
```

## Option 2: Skip the Warning (For Private Viewing/Testing)

Leave `App.vue` as is - the warning component exists but isn't used unless you add it.

## Why Include a Warning?

1. **Ethical responsibility**: Dark content needs context
2. **Legal protection**: Clear labeling as fiction
3. **Resource provision**: Help for those who need it
4. **Audience filtering**: Ensures informed consent

## Customizing the Warning

You can edit `ContentWarning.vue` to:
- Change the help resources (add international numbers)
- Modify the language/tone
- Add your film's title/production info
- Include trigger warnings specific to your content

The warning screen uses the same design system (lowercase, slow transitions, amber accents) but with a black background to clearly separate it from the "in-world" experience.
