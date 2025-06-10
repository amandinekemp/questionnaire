<template>
  <div class="question">
    <h3>{{ question.question }}</h3>
    <ul>
      <li v-for="(choice, index) in randomChoices" :key="choice">
        <Answer
            :id="'answer' + index"
            :name="'question' + question.question"
            :disabled="hasAnswer"
            :value="choice"
            v-model="answer"
            @change="onAnswer"
            :correctAnswer="question.correct_answer"
        />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'
import { shuffleArray } from '../functions/array'
import Answer from './Answer.vue'

const props = defineProps({
  question: Object
})
const emits = defineEmits(['answer'])

const answer = ref(null)
const hasAnswer = computed(() => answer.value !== null)

const randomChoices = ref([])
let timer

const onAnswer = () => {
  clearTimeout(timer)
  timer = setTimeout(() => {
    emits('answer', answer.value)
  }, 1_500)
}

onMounted(() => {
  randomChoices.value = shuffleArray(props.question.choices)

  timer = setTimeout(() => {
    if (!answer.value) {
      answer.value = ''
      onAnswer()
    }
  }, 30_000)
})

onUnmounted(() => {
  clearTimeout(timer)
})
</script>

<style>
.question {
  padding-top: 2rem;
}
</style>
