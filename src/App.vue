<script setup>
import { ref, watch } from "vue"
import VueDatePicker from "@vuepic/vue-datepicker"
import "@vuepic/vue-datepicker/dist/main.css"

const date = ref(null)
const startDate = ref(null)
const endDate = ref(null)

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
</script>

<template>
  <div class="p-5">
    <div class="bg-zinc-800 p-3 inline-block text-white font-bold shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
      <h1 class="text-2xl">ExitCalc</h1>
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
        <input type="number" value="24" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Додатковка відпустка:</label>
        <input type="number" value="7" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Використано днів відпустки:</label>
        <input type="number" placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>


      <div class="flex gap-2 flex-col">
        <label for="">Лікарняні дні:</label>
        <input type="number" placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>

      <div class="flex gap-2 flex-col">
        <label for="">Відпустка без збереження:</label>
        <input type="number" placeholder="0" class="w-full p-2 bg-[#212121]" />
      </div>

    </div>

    <div class="bg-zinc-800 mt-6 w-full h-full p-6 text-white shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
      <h2 class="text-2xl mb-4">Результат</h2>
      <p>Робочі дні: <span>———</span></p>
      <p>Щорічна компенсація: <span>———</span></p>
      <p>Додатковка компенсація: <span>———</span></p>
      <hr>
      <p>Формлуа щорічної компенсації</p>

      <p>Формула додаткової компенсації</p>
    </div>
  </div>
</template>

<style>
.custom-input {
  border: none;
  border-radius: 0;
}
</style>