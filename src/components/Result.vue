<script setup>
defineProps({
    vacationState: {
        type: Object,
        required: true
    }
})

const formatDate = (date) => {
    if (!date) return ""
    const d = new Date(date)
    const day = String(d.getDate()).padStart(2, "0")
    const month = String(d.getMonth() + 1).padStart(2, "0")
    const year = d.getFullYear()
    return `${day}.${month}.${year}`
}

function roundNum(number) {
    if (number === null) {
        return null;
    }
    return Math.round(number);
}
</script>

<template>
    <div class="bg-zinc-800 mt-6 w-full h-full p-6 flex flex-col gap-2 text-white shadow-[10px_10px_0_rgba(0,0,0,0.7)]">
        <h2 class="text-2xl">Результат</h2>
        <p>Дати: <span class="font-bold">{{ formatDate(vacationState.startDate) }}</span> — <span class="font-bold">{{
            formatDate(vacationState.endDate) }}</span></p>
        <p>Робочі дні: <span>{{ vacationState.daysWorking ? vacationState.daysWorking : "—" }}</span></p>
        <p>Щорічна компенсація: <span>{{ roundNum(vacationState.annualCompensation) }}</span></p>
        <p>Додатковка компенсація: <span>{{ roundNum(vacationState.additionalCompensation) }}</span></p>
        <hr>
        <p>Формлуа щорічної компенсації
            <br />
            ({{ vacationState.daysWorking ? vacationState.daysWorking : "Робочі дні" }} - {{
                vacationState.unpaidLeaveDays ? vacationState.unpaidLeaveDays : "Дні без збереження"}}) / 365 * {{
                vacationState.annual ? vacationState.annual : "Дні щорічної основної" }} -
            {{ vacationState.usedDays ? vacationState.usedDays : "Використано днів" }} = <span>{{
                vacationState.annualCompensation ?? 0 }}</span>
        </p>

        <p>Формула додаткової компенсації
            <br />
            {{ vacationState.additional ? vacationState.additional : "Додаткові дні" }} * ({{ vacationState.daysWorking
                ? vacationState.daysWorking : "Робочі дні" }} - {{
                vacationState.sickDays
                    ? vacationState.sickDays : "Лікарняні дні" }} - {{
                vacationState.unpaidLeaveDays ? vacationState.unpaidLeaveDays : "Дні без збереження" }}) / 365 = <span>{{
                vacationState.additionalCompensation ?? 0}}</span>
        </p>
    </div>
</template>