<script setup>
import CompentsCalc from './components/CompentsCalc.vue';
import Result from './components/Result.vue';
import CalcDate from './components/CalcDate.vue';
import { ref } from "vue"

const vacationState = ref({})
const mode = ref('compents')
</script>

<template>
  <div class="p-5">

    <div class="flex justify-between items-center mb-6">
      <div class="bg-zinc-800 p-3 inline-block text-white font-bold shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <h1 class="text-2xl">ExitCalc</h1>
      </div>

      <div class="bg-zinc-800 h-full p-3 inline-block text-white text-xl shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <select name="" id="" v-model="mode">
          <option value="compents">Компенсація</option>
          <option value="date">Розрахувати дату</option>
        </select>
      </div>
    </div>

    
    <CompentsCalc v-if="mode === 'compents'" v-model:state="vacationState"/>
    <Result v-if="mode === 'compents'" :vacationState="vacationState" />

    <CalcDate v-if="mode === 'date'"/>
  </div>
</template>

<style>
.custom-input {
  border: none;
  border-radius: 0;
}

.custom-input:focus {
  outline: solid;
}
</style>