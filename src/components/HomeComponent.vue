<template>
  <h3 class="main-header mb-5">Quiz App</h3>
<!--  <button @click="getData">click</button>-->
  <label>select category</label>
  <select class="form-select" v-model="selectedCategory">
    <option v-for="(item, index) in newResults" :key="index">{{item.category}}</option>
  </select>
  <br/>
  <label>Select Difficulty:</label>
  <select class="form-select" v-model="defficutlies">
    <option value="easy">easy</option>
    <option value="medium">medium</option>
    <option value="hard">hard</option>
  </select>
  <br/>
  <label>select type:</label>
  <select class="form-select" v-model="selectedType">
    <option value="multiple">multiple choice</option>
    <option value="boolean">true/false</option>
  </select>

  <a class="start-btn" @click="passEvents">start</a>
</template>

<script setup>
import {computed, defineEmits, defineProps, ref} from "vue";

const props = defineProps(['results'])
const emit = defineEmits(['showHome' , 'selectedCategory'])
console.log(props.results)

const selectedCategory = ref()
const defficutlies = ref()
const selectedType = ref()
// const filteredQuestions = ref()

const newResults = computed(() => {
  const cats = props.results.reduce((p,c) => {
    p[c.category] = c
    return p
  }, {})
  return Object.values(cats).sort((a,b) => a.category.localeCompare(b.category))
})




function passEvents(){
  emit('showHome')
  emit('selectedCategory', {selectedcategory : selectedCategory.value , defficutlies: defficutlies.value , selectedType : selectedType.value})
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.start-btn {
  width: 100%;
  background: #ff5c0b;
  display: block;
  margin: 30px 0;
  height: 50px;
  border-radius: 5px;
  text-align: center;
  justify-content: center;
  align-items: center;
  display: flex;
  text-decoration: none;
  color: #fff;
  font-weight: 900;
}
</style>
