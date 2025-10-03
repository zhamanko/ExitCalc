<script setup>
import { ref, reactive, watchEffect, watch } from "vue"
import VueDatePicker from "@vuepic/vue-datepicker"
import "@vuepic/vue-datepicker/dist/main.css"

const state = reactive({
    startDate: null,
    endDate: null,
    daysWorking: null,
    annual: 24,
    additional: 7,
    usedDays: null,
    sickDays: null,
    unpaidLeaveDays: null,
    annualCompensation: "―",
    additionalCompensation: "―"
})

const date = ref(null)

const emit = defineEmits(["update:state"])

watchEffect(() => {
    if (state.startDate && state.endDate) {
        state.daysWorking = calculateWorkDays(state.startDate, state.endDate)
    } else {
        state.daysWorking = null
    }

    state.annualCompensation = calculatorAnnual(state.usedDays ?? 0, state.annual ?? 0, state.unpaidLeaveDays ?? 0, state.daysWorking)
    state.additionalCompensation = calculatorAdditional(
        state.additional ?? 0,
        state.sickDays ?? 0,
        state.unpaidLeaveDays ?? 0,
        state.daysWorking
    )

    emit("update:state", { ...state })
})

watch(date, (newDate) => {
    if (!newDate) {
        state.startDate = null
        state.endDate = null
        return
    }
    if (Array.isArray(newDate)) {
        state.startDate = new Date(newDate[0])
        state.endDate = new Date(newDate[1])
    }
})

function calculateWorkDays(dateStart, dateEnd) {
    const start = new Date(dateStart)
    const end = new Date(dateEnd)
    if (end < start) return "Дата закінчення не може бути раніше дати початку"
    const diffTime = Math.abs(end - start)
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1
    return diffDays > 0 ? diffDays : null;
}

function calculatorAnnual(usedDays, vacationPerYear, unpaidLeaveDays, days) {
    if (days === null || days === 0) return null
    const compensation = ((days - unpaidLeaveDays) / 365) * vacationPerYear - usedDays
    return compensation >= 0 ? compensation.toFixed(2) : null
}

function calculatorAdditional(additionalDays, sickDays, unpaidLeaveDays, days) {
    if (days === null || days === 0) return null
    const compensation = additionalDays * (days - sickDays - unpaidLeaveDays) / 365
    return compensation >= 0 ? compensation.toFixed(2) : null
}
</script>

<template>

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
            <input type="number" v-model="state.annual" value="24" class="w-full p-2 bg-[#212121]" />
        </div>

        <div class="flex gap-2 flex-col">
            <label for="">Додатковка відпустка:</label>
            <input type="number" v-model="state.additional" value="7" class="w-full p-2 bg-[#212121]" />
        </div>

        <div class="flex gap-2 flex-col">
            <label for="">Використано днів відпустки:</label>
            <input type="number" v-model="state.usedDays" placeholder="0" class="w-full p-2 bg-[#212121]" />
        </div>


        <div class="flex gap-2 flex-col">
            <label for="">Лікарняні дні:</label>
            <input type="number" v-model="state.sickDays" placeholder="0" class="w-full p-2 bg-[#212121]" />
        </div>

        <div class="flex gap-2 flex-col">
            <label for="">Відпустка без збереження:</label>
            <input type="number" v-model="state.unpaidLeaveDays" placeholder="0" class="w-full p-2 bg-[#212121]" />
        </div>

    </div>
</template>

<style scoped>
.custom-input {
    border: none;
    border-radius: 0;
}

.custom-input:focus {
    outline: solid;
}
</style>