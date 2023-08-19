<template>
 <div class="final-container">
   <h2 class="message">{{ message }}</h2>
   <div class="answer-results">
     <div class="bar">
       <p class="count">{{correctAnswers}}/{{numberOfQuestions}}</p>
       <span :style="{ width: `${correctAnswersPercentage}%` }"
             class="correct"></span>
       <span :style="{ width: `${wrongAnswersPercentage}%` }"
             class="wrong"></span>
     </div>


   </div>
   <button class="restart-btn" @click="restartGame">restart</button>
   <!--  <p>upload vue app on guthub page / upload on ftp serve / complete quiz-app / erroe music app/ learn vue-x</p>-->

 </div>

</template>

<script setup>
import {defineEmits, defineProps, onMounted, ref} from "vue";

const emit = defineEmits(['restart'])
const props = defineProps(['numberOfQuestions', 'correctAnswers'])
console.log(props)
const message = ref('')
const correctAnswersPercentage = ref(0)
const wrongAnswersPercentage = ref(0)

function calcutePercentage(){
  correctAnswersPercentage.value = (props.correctAnswers / props.numberOfQuestions) * 100
  console.log(correctAnswersPercentage.value+ '%')
  wrongAnswersPercentage.value = (100 - correctAnswersPercentage.value)
  console.log(wrongAnswersPercentage.value + '%')
}


function restartGame() {
  emit('restart')
}

function textresult(){
  if (correctAnswersPercentage.value >= 0 && correctAnswersPercentage.value <= 30){
    message.value = `you only got ${correctAnswersPercentage.value}% of the question right. you should try again ...`
  }
  if (correctAnswersPercentage.value > 30 && correctAnswersPercentage.value < 60){
    message.value = `you only got ${correctAnswersPercentage.value}% of the question right. you can get a better score playing again`
  }
  if (correctAnswersPercentage.value > 60 && correctAnswersPercentage.value < 80) {
    message.value = `You got ${correctAnswersPercentage.value}% of the questions right. That's a good score!`
  }
  if (correctAnswersPercentage.value >= 80 && correctAnswersPercentage.value < 95) {
    message.value = `You got ${correctAnswersPercentage.value}% of the questions right! very good`
  }
  if (correctAnswersPercentage.value >= 80 && correctAnswersPercentage.value < 95) {
    message.value = `You got ${correctAnswersPercentage.value}% of the questions right! very good`
  }
  if (correctAnswersPercentage.value >= 95) {
    message.value = `You got ${correctAnswersPercentage.value}% of the questions right! Congratulations`
  }

}
onMounted(() =>{
  calcutePercentage()
  textresult()
})


</script>

<style scoped lang="scss">
.restart-btn {
  background-color: #eec502;
  padding: 10px 20px;
  color: #fff;
  border-radius: 15px;
}
.bar {
  background: rgb(40, 39, 49);
  box-shadow: 0px 0px 4px #2e2e4a;
  margin: 30px auto;
  width: 50%;
  height: 30px;
  border-radius: 5px;
  overflow: hidden;
  .correct {
  background-color: green;
    height: 100%;
    display: inline-block;
    order-radius: 5px;
}
  .wrong {
    background-color: red;
    height: 100%;
    display: inline-block;
    order-radius: 5px;
  }
}
.count {
  position: absolute;
  right: 50%;
}
.final-container {
  padding: 100px;
  text-align: center;
}

</style>
