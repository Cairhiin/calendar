<template>
    <div class="mt-8 bg-stone-100 p-4 rounded text-stone-800">
        <h3 class="text-bold text-xl">Afspraken en taken vandaag</h3>
        <div v-if="!calendarItems.length" class="my-5">Geen afspraken of taak deadlines vandaag</div>
        <div v-for="item in calendarItems" class="rounded my-4 p-2 border-l-8" :class="{
            'border-sky-800/75 bg-sky-800/5': item.type === 'meeting',
            'border-green-800/75 bg-green-800/5': item.type === 'todo',
            'border-red-800/75 bg-red-800/5':
                item.type === 'todo' && !(new Date(item.ends_at) > new Date() && !item.data_completed)
        }">
            <div class="font-bold uppercase">{{ item.title }}</div>
            <div class="text-stone-900/75">{{ item.description }}</div>
            <div class="mt-2 text-stone-900/75">
                <div class="capitalize">Starttijd: {{ item.starts_at.toLocaleDateString('nl-NL', {
                    weekday: 'long',
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                }) }} ({{
    item.starts_at.toLocaleTimeString('nl-NL') }})</div>
                <div class="capitalize">Eindtijd: {{ item.ends_at.toLocaleDateString('nl-NL', {
                    weekday: 'long',
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                }) }} ({{
    item.ends_at.toLocaleTimeString('nl-NL') }})</div>
            </div>
        </div>
        <div class="flex gap-4 uppercase text-white font-bold mt-8 text-xs md:text-sm">
            <div class="flex-1 bg-sky-800/75 p-2 text-center rounded">Afspraak</div>
            <div class="flex-1 bg-green-800/75 p-2 text-center rounded">Openstaande taak</div>
            <div class="flex-1 bg-red-800/75 p-2 text-center rounded">Te late taak</div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        calendarItems: Object
    }
}
</script>