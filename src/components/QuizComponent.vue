<template>
  <div class="py-5">
    <div class="container">
      <h3 class="text-center mb-3">{{ apiData.title }}</h3>
      <div class="text-center fw-bold" v-show="result !== 0">Result: {{ result }}</div>
      <form action="">
        <div class="card m-2" v-for="(question, index) in apiData.data" :key="question.id"
          :class="submit ? selectedAnswer[index]?.correct ? 'correctClass' : 'wrongClass' : 'notSubmit'">
          <div class=" card-body">
            {{ question.id }}. {{ question.question }}
            <div class="form-check small ms-4" v-for="choice in question.answers" :key="choice">
              <input class="form-check-input" type="radio" :id="choice" :value="choice" :name="question.answers"
                @change="answered($event, question.id)">
              <label class="form-check-label" :for="choice">
                {{ choice }}
              </label>
            </div>
            <div v-if="submit ? selectedAnswer[index]?.correct ? message = 'correct' : message = 'wrong' : ''"
              class="text-end">
              <span class="small" :class="message === 'correct' ? 'text-success' : 'text-danger'">
                {{ message }}
              </span>
            </div>
          </div>
        </div>
      </form>
      <div class="text-center mt-3">
        <button class="btn btn-primary " type="button" v-on:click.prevent="submitQuiz()">Submit
          Solution</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Quiz from '../assets/data/question.json';

let quiz = ref(Quiz);
let apiData = ref(quiz.value);
let selectedAnswer = ref([]);
let result = ref(0)
let message = ref('');
let isRight = ref(false)
let submit = ref(false);

const answered = function (event, id) {
  const data = {
    id: id,
    answer: event.target.value
  }
  submit = false;
  result.value = 0;
  message = '';
  const i = selectedAnswer.value.findIndex((_element) => data.id === _element.id);

  if (i > -1) selectedAnswer.value[i] = data;
  else selectedAnswer.value.push(data);

  let selectedAnswers = selectedAnswer.value.sort((a, b) => a.id - b.id);
  const dbData = apiData.value.data;

  dbData.map((value, index) => {
    if (value?.id === selectedAnswers[index]?.id) {
      if (value?.correct_answer === selectedAnswers[index]?.answer) {
        selectedAnswers[index].correct = true
        isRight = true
      }
      else {
        isRight = false;
        selectedAnswers[index].correct = false
      }
    }
  })
}

const submitQuiz = () => {
  submit = true;
  const trueCount = selectedAnswer.value.filter(ans => ans.correct === true)
  result.value = parseInt((trueCount.length / apiData.value.data.length) * 100) + '%'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

.correctClass {
  border: #42b983 2px solid
}

.wrongClass {
  border: red 2px solid
}

.notSubmit {
  border: #d5df1b 2px solid
}
</style>
