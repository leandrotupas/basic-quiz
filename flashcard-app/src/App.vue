<script setup>
  import { ref, computed } from 'vue'

  const questions = ref([
    {
      question: "What is Vue JS?",
      answer: "Framework",
      options: [
        'Framework',
        'Library',
        'Programming Language'
      ],
      selected: null
    },
    {
      question: "2+2?",
      answer: "4",
      options: [
        '1',
        '2',
        '4'
      ],
      selected: null
    },
    {
      question: "Ariana's Latest Release?",
      answer: "Eternal Sunshine",
      options: [
        'Eternal Sunshine',
        'Positions',
        'Thank You Next'
      ],
      selected: null
    },
  ])

  const quizCompleted = ref(false)

  const currentQuestion = ref(0)

  const score = computed(() => {
    let value = 0
    questions.value.map(q => {
      if (q.selected == q.answer) {
        value++
      }
    })
    return value
  })

  const getCurrentQuestion = computed(() => {
    let question = questions.value[currentQuestion.value]
    question.index = currentQuestion.value
    return question
  })

  const setAnswer = evt => {
    questions.value[currentQuestion.value].selected = evt.target.value
    evt.target.value = null
  }

  const nextQuestion = () => {
    if (currentQuestion.value < questions.value.length - 1) {
      currentQuestion.value++
    } else {
      quizCompleted.value = true
    }
  }
</script>

<template>
  <main class="app">
    <h1> Quiz </h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question"> {{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }} / {{ questions.length }}</span>
      </div>

      <div class="options">
        <label 
          v-for="(option, index) in getCurrentQuestion.options" 
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected === option
              ? option === getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected !== null && getCurrentQuestion.selected !== option
              ? 'disabled'
              : ''
          }`">

          <input 
            type="radio" 
            :name="getCurrentQuestion.index"
            :value="option"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected !== null && getCurrentQuestion.selected !== option"
            @change="setAnswer">
          <span> {{ option }} </span>
        </label>
      </div>


      <button 
        @click="nextQuestion"
        :disabled = "!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? 'Finish'
            : getCurrentQuestion.selected == null
              ? 'Select an Option'
              : 'Next Question'

        }}
      </button>
    </section>
      
    <section v-else>
      <h2> You have finished the quiz!</h2>
      <p> Your score is {{ score }} / {{ questions.length }}</p>
    </section>

  </main>
</template>

<style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, Helvetica, sans-serif;
  }

  body {
    color: white;
  }

  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    min-height: 100vh;
    background-color: #134611;
  }

  h1 {
    font-size: 2rem;
    margin-bottom: 2rem;
    color: white;
  }

  .quiz {
    background-color: #E8FCCF;
    padding: 1rem;
    width: 100%;
    max-width: 640px;
    border-radius: 0.5rem;
  }

  .quiz-info {
    margin-bottom: 1rem;
    font-size: 1.5rem;
    justify-content: space-between;
    display: flex;
    color:#134611;
  }

  .quiz-info .score{
    background-color: #3DA35D;
    padding: 0.5rem;
    border-radius: 0.5rem;
    color: white;
  }

  .option {
    display: block;
    padding: 1rem;
    background-color: #3DA35D;
    border-radius: 0.5rem;
    margin-bottom: 0.5rem;
    cursor: pointer;
  }

  .option:hover {
    background-color: #3E8914;
  }

  .option.correct {
    background-color: #96E072;
  }

  .option.wrong {
    background-color: brown;
  }

  .option:last-of-type{
    margin-bottom: 0;
  }

  .option.disabled {
    opacity: 0.5;
  }

  .option input {
    display: none;
  }

  button {
    appearance: none;
    outline: none;
    border: none;
    cursor: pointer;

    padding: 0.5rem 1rem;
    background-color: #3DA35D;
    color: white;
    font-weight: 200;
    font-size: 1.2rem;
    margin-top: 1rem;
    border-radius: 0.5rem;
  }

  button:disabled {
    opacity: 0.5;
  }

  h2 {
    font-size: 2rem;
    margin-bottom: 2rem;
    text-align: center;
    color: white;
  }

  p {
    color: #134611;
    font-size: 1.25rem;
    text-align: center;
    color: gainsboro;
  }
</style>
