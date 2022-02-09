<template>
  <div class="container mx-auto h-screen">
    <h1 class="text-center py-10 text-5xl">Breakpoints Maker</h1>
    <div class="flex justify-center items-end my-20 space-x-5">
      <div class="w-40">
        <label for="baseValue">Breakpoint</label>
        <input
          type="number"
          id="baseValue"
          class="border-2 w-full text-center border-gray-800 rounded px-2 py-1 text-5xl focus:outline-0"
          placeholder="px/em"
          v-model="baseValue"
          @keypress.enter="calculateResult()"
        >
      </div>
      <div class="w-40">
        <label for="inputValue">Any css size</label>
        <input
          type="number"
          id="inputValue"
          class="border-2 text-center border-gray-800 rounded px-2 py-1 text-5xl w-40 focus:outline-0"
          placeholder="px/em"
          v-model="inputValue"
          @keypress.enter="calculateResult()"
        >
      </div>
      <button
        class="text-xl h-[68px] tracking-wider rounded text-white w-40"
        @click="calculateResult()"
        :class="{'bg-gray-600':!inputValue || !baseValue,'bg-gray-800':inputValue && baseValue}"
        :disabled="!inputValue || !baseValue"
      >Calculate</button>
      <button
        class="text-xl h-[68px] tracking-wider rounded text-white w-40"
        @click="clearResult()"
        :class="{'bg-gray-600':!inputValue || !baseValue,'bg-gray-800':inputValue && baseValue}"
        :disabled="!inputValue || !baseValue"
      >Clear</button>
    </div>
    <div class="flex justify-center p-10 items-center">
      <div
        class="flex flex-col border-r last:border-r-0 space-y-5 border-gray-800 py-4 px-16"
        v-for="breakPoint,index in breakPoints"
        :key="index"
      >
        <p class="text-gray-800 text-3xl"><span class="mr-3 text-lg">{{breakPoint.key}}</span>{{breakPoint.value}}{{breakPoint.unit}}</p>
        <p class="text-5xl text-center font-bold">{{results[breakPoint.key]}}</p>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'

const breakPoints = [
  { key:'sm', value : 640, unit : 'px' },
  { key:'md', value : 768, unit : 'px' },
  { key:'lg', value : 1024, unit : 'px' },
  { key:'xl', value : 1280, unit : 'px' },
  { key:'2xl', value : 1536, unit : 'px' },
];

const results = ref({
  sm:'',
  md:'',
  lg:'',
  xl:'',
  '2xl':''
});

const inputValue = ref('')
const baseValue = ref('')

function calculateResult(){
  if(!inputValue.value){
    return;
  }
  if(!baseValue.value){
    return;
  }
  results.value = {
    sm:'',
    md:'',
    lg:'',
    xl:'',
    '2xl':''
  }
  breakPoints.forEach(breakPoint => {
    console.log(breakPoint)
    let px = parseInt(inputValue.value ?? 0);
    let bp = parseInt(baseValue.value ?? 0);
    results.value[breakPoint.key] = Math.round((px * breakPoint.value) / bp);
  });
}

function clearResult(){
  inputValue.value = '';
  baseValue.value = '';
  results.value = {
    sm:'',
    md:'',
    lg:'',
    xl:'',
    '2xl':''
  };
}



</script>
<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>