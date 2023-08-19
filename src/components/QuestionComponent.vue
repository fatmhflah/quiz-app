<template>
  <div>
    <h2 class="question">{{index + 1}} - {{filteredQuestions[index].question}}</h2>
    <div class="timer">
      <span class="clock"></span>
      <p>0:{{ secondsLeft }}</p>
    </div>
    <div class="answers row">
      <div class="col-lg-5 box" v-html="question1" @click="checkAnswer($event)" :style="[roundEnded ? '' : { background: '#fff' }]">
      </div>
      <div class="col-lg-5 box" v-html="question2" @click="checkAnswer($event)" :style="[roundEnded ? '' : { background: '#fff' }]">
      </div>
      <div class="col-lg-5 box" v-html="question3" @click="checkAnswer($event)" :style="[roundEnded ? '' : { background: '#fff' }]">
      </div>
      <div class="col-lg-5 box" v-html="question4" @click="checkAnswer($event)" :style="[roundEnded ? '' : { background: '#fff' }]">
      </div>
    </div>
    <p>{{message}}</p>
    <button class="btn next-question" @click="nextQuestion"  v-html="[
          index == numberOfQuestions - 1 ? 'See results!' : 'Next question',
        ]"></button>

  </div>

</template>

<script setup>
import {defineEmits, defineProps, onMounted, ref} from "vue";
const props = defineProps({
  filteredQuestions : Array
})
const emit = defineEmits(['endGame'])
console.log(props.filteredQuestions)

const index = ref(0)
const answers = ref([])
const gameFinished = ref(false)
const question1 = ref('')
const question2 = ref('')
const question3 = ref('')
const question4 = ref('')
const correctAnswers = ref(0)
const isUserAnswerCorrect = ref(undefined)
const message = ref('')
const correctAnswer = ref()
const userAnswered = ref(false)
const roundEnded =  ref(false)
const secondsLeft = ref(0)
const numberOfQuestions = ref(0)
const timer = ref()

function getRandomAnswer() {
  const randomNumber = Math.floor(Math.random() * answers.value.length);
  let answer = answers.value.splice(randomNumber, 1)[0];
  console.log(answer)
  return answer;
}
function countDown(){
  secondsLeft.value = 45
  timer.value = setInterval(() => {
    if (!userAnswered.value){
      if (secondsLeft.value > 0) secondsLeft.value--
      else if (secondsLeft.value <= 0 || roundEnded.value) stopTimer()
    }
    if (secondsLeft.value <= 0 && !userAnswered.value) {
      message.value = "Time ran out! No points in this round.";
      roundEnded.value = true;
    }
  },1000)
}
function stopTimer() {
  clearInterval(timer.value);
}

function prepareQuestion() {
  numberOfQuestions.value = props.filteredQuestions.length

    answers.value.push(props.filteredQuestions[index.value].correct_answer);
    console.log(answers.value)
    props.filteredQuestions[index.value].incorrect_answers.forEach((answer) => {
      answers.value.push(answer);
    });
    correctAnswer.value = ref(props.filteredQuestions[index.value].correct_answer)
    console.log(correctAnswer)
    question1.value = getRandomAnswer()
    question2.value = getRandomAnswer()
    question3.value = getRandomAnswer()
    question4.value = getRandomAnswer()

    countDown()
}

onMounted(async() =>{
    prepareQuestion()
})

function checkAnswer(event) {
  if (!roundEnded.value){
    roundEnded.value = true
    userAnswered.value = true

    console.log(event.target.innerText)
    console.log(correctAnswer.value.value)
    isUserAnswerCorrect.value = event.target.innerText == correctAnswer.value.value ? true : false;
    console.log(isUserAnswerCorrect.value)

    message.value = isUserAnswerCorrect.value
        ? "Good job! 🐢"
        : "Better luck next time.";

    if (isUserAnswerCorrect.value){correctAnswers.value++}
    console.log(correctAnswer.value)

    setTimeout(() => {
      event.target.style.background = isUserAnswerCorrect.value
          ? "#00e900"
          : "#ff4a4a";
    }, 200);
  }

}
function nextQuestion(){
  checkIfItIsTheLastRound()
  if (!gameFinished.value){
    index.value++
    roundEnded.value = false
    userAnswered.value = false
    isUserAnswerCorrect.value = undefined
    answers.value = []
    message.value = ''
    prepareQuestion()
  }

}
function checkIfItIsTheLastRound() {
  console.log(index.value)
  console.log(numberOfQuestions.value - 1)
  if (index.value == numberOfQuestions.value - 1) {
    gameFinished.value = true;
    emit("endGame", {numberOfQuestions: numberOfQuestions.value, correctAnswers: correctAnswers.value
    });
    console.log('endGame')
  }
}

</script>

<style scoped lang="scss">
.question {
  margin-bottom: 30px;
}
.box {
  display: flex;
  background: #fff;
  color: #000;
  align-items: center;
  justify-content: center;
  height: 100px;
  border-radius: 15px;
  margin: 15px;
}
.next-question {
  background: #f3d543;
  color: #fff;
  padding: 15px;
  border-radius: 15px;
  width: 88%;
  margin-top: 30px;
  &:hover {
  background-color: #eec502;
   }
}
</style>
