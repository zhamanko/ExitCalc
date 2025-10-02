<script setup>
import { ref, watch } from "vue"
import VueDatePicker from "@vuepic/vue-datepicker"
import "@vuepic/vue-datepicker/dist/main.css"

const date = ref(null)

const startDate = ref(null)
const endDate = ref(null)

const days = ref("―")
const annualCompensation = ref("―")
const additionalCompensation = ref("―")

const annual = ref(24)
const additional = ref(7)
const usedDays = ref(null)
const sickDays = ref(null)
const unpaidLeaveDays = ref(null)

const formatDate = (date) => {
  if (!date) return ""
  const d = new Date(date)
  const day = String(d.getDate()).padStart(2, "0")
  const month = String(d.getMonth() + 1).padStart(2, "0")
  const year = d.getFullYear()
  return `${day}.${month}.${year}`
}

watch(date, (newDate) => {
  if (!newDate) {
    startDate.value = null
    endDate.value = null
    return
  }
  if (Array.isArray(newDate)) {
    startDate.value = formatDate(newDate[0])
    endDate.value = formatDate(newDate[1])
  } else {
    startDate.value = formatDate(newDate)
    endDate.value = null
  }
})

function calculatorAnnual(usedDays, vacationPerYear) {
  const compensation = (this.days / 365) * vacationPerYear - usedDays;
  return compensation >= 0 ? compensation.toFixed(2) : "――";
}

function calculatorAdditional(additionalDays, sickDays, unpaidLeaveDays) {
  const compensation = additionalDays * (this.days - sickDays - unpaidLeaveDays) / 365;
  return compensation >= 0 ? compensation.toFixed(2) : "――";
}

function roundNum(number) {
  if (number === "――") {
    return "――";
  }
  return Math.round(number);
}
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

    <div class="bg-zinc-800 mt-6 w-full h-full flex flex-col gap-4 p-6 text-white shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
      <div class="flex gap-2 flex-col">
        <label for="date" class="">
          Дати
        </label>
        <VueDatePicker v-model="date" mode="date" range :partial-range="true" :enable-time="false"
          :enable-time-picker="false" :format="'dd.MM.yyyy'" dark locale="uk" arrow-navigation
          :ui="{ input: 'custom-input' }" placeholder="Введіть дату" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Щорічна відпустка:</label>
        <input type="number" v-model="annual" value="24" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Додатковка відпустка:</label>
        <input type="number" v-model="additional" value="7" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Використано днів відпустки:</label>
        <input type="number" v-model="usedDays"  placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>


      <div class="flex gap-2 flex-col">
        <label for="">Лікарняні дні:</label>
        <input type="number" v-model="sickDays" placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Відпустка без збереження:</label>
        <input type="number" v-model="unpaidLeaveDays" placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>

    </div>

    <div class="bg-zinc-800 mt-6 w-full h-full p-6 flex flex-col gap-2 text-white shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
      <h2 class="text-2xl">Результат</h2>
      <p>Дати: <span class="font-bold">{{ startDate }}</span> — <span class="font-bold">{{ endDate }}</span></p>
      <p>Робочі дні: <span>{{ days }}</span></p>
      <p>Щорічна компенсація: <span>{{ annualCompensation }}</span></p>
      <p>Додатковка компенсація: <span>{{ additionalCompensation }}</span></p>
      <hr>
      <p>Формлуа щорічної компенсації
        <br />
        Робочі дні / 365 * {{ annual ? annual : "Дні щорічної основної" }} - {{ usedDays ? usedDays : "Використано днів" }}
      </p>

      <p>Формула додаткової компенсації
        <br />
        {{ additional  ? additional : "Додаткові дні" }} * (Робочі дні - {{ sickDays ? sickDays : "Лікарняні дні"}} - {{ unpaidLeaveDays ? unpaidLeaveDays : "Дні без збереження"}}) / 365
      </p>
    </div>
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