<script setup>
import { ref } from "vue"

import VueDatePicker from "@vuepic/vue-datepicker"
import "@vuepic/vue-datepicker/dist/main.css"

const date = ref(null)
const days = ref(null)

function calculateDate(date, days) {
    if (!this.date || !this.days || isNaN(this.days)) {
        return '――';
    }

    const strDay = ['Неділя', 'Понеділок', 'Вівторок', 'Середа', 'Четвер', "П'ятниця", 'Субота'];
    const start = new Date(this.date);
    
    if (isNaN(date.getTime()) || this.days < 1) {
        return '――';
    }

    start.setDate(start.getDate() + (days - 1));
    return start.toLocaleDateString('uk-UA') + " | " + strDay[start.getDay()];
}
</script>

<template>
    <div class="bg-zinc-800 p-6 inline-block w-full text-white shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <div class="flex gap-2 flex-col">
            <label for="date" class="">
                Дати
            </label>
            <VueDatePicker v-model="date" mode="date" :enable-time="false" :enable-time-picker="false"
                :format="'dd.MM.yyyy'" dark locale="uk" text-input arrow-navigation :ui="{ input: 'custom-input' }"
                placeholder="Введіть дату" />
        </div>
        <div class="flex gap-2 flex-col mt-4">
            <label for="">Календарні дені</label>
            <input type="nubmer" v-model="days" placeholder="Введіть календарні дні" class="w-full p-2 bg-[#212121]">
        </div>
        <div>
            <p class="mt-4 text-center">Дата: {{ calculateDate(date, days) }}</p>
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