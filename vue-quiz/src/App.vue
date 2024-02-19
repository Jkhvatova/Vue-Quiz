<script setup>
import { ref, computed } from 'vue';

const questions = ref([
{
      question: "What is the CPU of a computer responsible for?",
      options: ["Making coffee", "Doing the laundry", "Processing instructions"],
      correctAnswer: 2,
      selected: null,
    },
    {
      question: "Which type of memory is non-volatile and retains its data when the power is turned off?",
      options: ["ROM", "ROOM", "WROOM"],
      correctAnswer: 0,
      selected: null,
    },
    {
      question: "What does HTML stand for?",
      options: ["Hippo Tiger Magic Land", "Hot Teapot Making Laugh", "Hyper Text Markup Language"],
      correctAnswer: 2,
      selected: null,
    },
    {
      question: "Which programming language is known for its use in web development?",
      options: ["English", "Hindi", "JavaScript"],
      correctAnswer: 2,
      selected: null,
    },
    {
      question: "What is the function of a firewall in computer security?",
      options: ["Shielding from cat attacks", "Filtering incoming and outgoing network traffic", "Making fireballs"],
      correctAnswer: 1,
      selected: null,
    }

])
const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.correctAnswer) {
      value++;
    }
  })
  return value;
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
})
const setAnswer = e => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
}

const nextQuestion = () => {
  currentQuestion.value++;
  if (currentQuestion.value >= questions.value.length) {
    quizCompleted.value = true;
  }
}

const restartQuiz = () => {
  questions.value.forEach((q) => {
    q.selected = null;
  });
  currentQuestion.value = 0;
  quizCompleted.value = false;
}
</script>

<template>
  <main class="app">
<h1>Vue Quiz</h1>
<section class="quiz" v-if="!quizCompleted">
  <div class="quiz-content">
    <div class="question">{{ getCurrentQuestion.question }}</div>
    <div class="score">Score {{ score}} / {{ questions.length }}</div>
  </div>
    <div class="options">
      <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
             getCurrentQuestion.selected == index
               ? index == getCurrentQuestion.correctAnswer
                 ? 'correct' : 'wrong'
                  : ''
      } ${
        getCurrentQuestion.selected != null
         && index != getCurrentQuestion.selected ? 'disabled'
         : ''
        }
          `">
        <input type="radio"
        :id="'option' + index"
        :name="getCurrentQuestion.index"
        :value="index"
        v-model="getCurrentQuestion.selected"
        :disabled="getCurrentQuestion.selected"
         @change="setAnswer"
        >
        <span>{{ option }}</span>
      </label>
    </div>
  <button
    @click="nextQuestion"
    :disabled="!getCurrentQuestion.selected">
      {{
        getCurrentQuestion.index == questions.length - 1 ? 'Finish' : getCurrentQuestion.selected == null ? 'Select an answer' : 'Next Question'}}</button>
</section>
<section v-else>
  <h2 v-if="score === questions.length">Congratulations! You know everything!</h2>
  <h2 v-else-if="score >= questions.length / 2">Well done! But maybe want to repeat?</h2>
  <h2 v-else>Did you even try to pass the quiz?</h2>
  <p class="end-text">Your score is {{ score }} / {{ questions.length }}</p>
  <button @click="restartQuiz">Restart Quiz</button>
</section>
</main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}
body {
  background-color: #023652;
  color: #fff;
}
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}
h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}
h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
  color: #8f8f8f;

}
.quiz {
  background-color: #0f577e;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}
.quiz-content {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}
.question {
  color: #adabab;
  font-size: 1.3rem;
  padding-right: 2rem;
}
.score {
  color: #c0bdbd;
  font-size: 1.2rem;
  font-weight: 700;
  text-wrap: nowrap;
}
.options {
  margin-bottom: 1rem;
}
.option {
  display: block;
  padding: 1rem;
  background-color: #083247;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.option:hover {
  background-color: #093a52;
}
.option.correct {
  background-color: #037f51;
}
.option.wrong {
  background-color: #973622;
}
.option:last-of-type {
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
  background-color: #0d8078;
  color: #083247;;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}
.button:disabled {
  opacity: 0.5;
}
.end-text {
  padding: 1rem;
  font-size: 1.2rem;
  text-align: center;
  color: #adabab;
}
</style>
