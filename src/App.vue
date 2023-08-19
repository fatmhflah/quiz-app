<template>
  <div class="container">
    <home-component @showHome="startGame"  @selectedCategory="selectedCategory($event)" v-if="showHome" :results="results"></home-component>
    <question-component v-if="gameStarted && results  && !gameFinished" :filteredQuestions="filteredQuestions" @endGame="endGame($event)"></question-component>
    <final-component v-if="gameFinished" @restart="restart" :correctAnswers="correctAnswers" :numberOfQuestions="numberOfQuestions"></final-component>
    <unavailabe-component v-if="unavailable" @back="back"></unavailabe-component>

  </div>

</template>

<script setup>
import {onMounted, ref} from "vue";
import HomeComponent from "@/components/HomeComponent.vue";
import QuestionComponent from "@/components/QuestionComponent.vue";
import FinalComponent from "@/components/FinalComponent.vue";
import UnavailabeComponent from "@/components/UnavailabeComponent.vue";
import axios from "axios";
import {BASE_URL} from "@/apis/models";


const showHome = ref(true)
const results = ref([])
const index = ref(0)
const unavailable = ref(false)
const filteredQuestions = ref()
const numberOfQuestions = ref(0)
const correctAnswers = ref(0)
const gameStarted = ref(false)
const gameFinished = ref(false)

onMounted(async () =>{
  axios.get(`${BASE_URL}/results`).then(response => {
    results.value = response.data
    console.log(results.value[index.value].category)
    // console.log(results.value)


  }).catch(error => {
    console.error('error' , error)
  })
})
function startGame(){
  showHome.value = false
  gameStarted.value = true


}
function selectedCategory(data){
  console.log(data.selectedType)
  filteredQuestions.value = results.value.filter(item => item.category == data.selectedcategory && item.difficulty == data.defficutlies && item.type == data.selectedType)
  console.log(filteredQuestions)

  if (filteredQuestions.value.length == 0){
    gameStarted.value= false
    unavailable.value = true
  }
}

function endGame(data) {
  console.log(data.numberOfQuestions)
  console.log(data.correctAnswers)
  numberOfQuestions.value = data.numberOfQuestions;
  correctAnswers.value = data.correctAnswers;
  gameFinished.value = true;
}
function restart(){
  showHome.value = true
  gameStarted.value = false;
  gameFinished.value = false;
  numberOfQuestions.value = 0;
}
function back(){
  console.log('backkk')
  showHome.value = true
  unavailable.value = false
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  margin-top: 60px;
}

body {
  background-color: #273550!important;
  color: #fff!important;
}
.form-select:focus {
  box-shadow: none!important;
}
p {
  margin: 0;
}
</style>
