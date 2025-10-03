<script setup>
import CompentsCalc from './components/CompentsCalc.vue';
import Result from './components/Result.vue';
import { ref } from "vue"

const vacationState = ref({})
</script>

<template>
  <div class="p-5">

    <div class="flex justify-between items-center">
      <div class="bg-zinc-800 p-3 inline-block text-white font-bold shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <h1 class="text-2xl">ExitCalc</h1>
      </div>

      <div class="bg-zinc-800 h-full p-3 inline-block text-white text-xl shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <select name="" id="">
          <option value="">Компенсація</option>
          <option value="">Розрахувати дату</option>
        </select>
      </div>
    </div>

    <CompentsCalc v-model:state="vacationState"/>

    <Result :vacationState="vacationState" />
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