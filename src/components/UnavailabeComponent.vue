<template>
  <div class="text-center">
    <h2 class="alert alert-warning text-center">Ooops there isnt any question !!!</h2>
    <button class="back mt-5" @click="back">back Home</button>
  </div>

</template>

<script setup>
import {defineEmits} from "vue";

const emit = defineEmits(['back'])
function back(){
  emit('back')
}
</script>

<style scoped>
.back {
  background-color: #eec502;
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
}
</style>
