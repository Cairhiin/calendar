<template>
    <div class="grid grid-cols-[50px_repeat(7,_1fr)] items-start">
        <div class="w-20">

        </div>
        <div v-for="day in week" :key="day" :class="{
            'border-r': day % 7 !== 0
        }" class="relative text-center capitalize flex-1 bg-stone-200 p-2 border-stone-100">{{
    getDayOfTheWeek(day, 'nl-NL')
}} <span class="text-stone-700">{{ getDate(day) }}</span></div>

        <template v-for="hour in day">
            <div class="py-2 pr-2 text-right -mt-4">{{ hour - 1 < 10 ? `0${hour - 1}:00` : `${hour - 1}:00` }}</div>
                    <template v-for="day in week">
                        <div class="border-b border-stone-200" :class="{
                            'border-r': day % 7 !== 0,
                            'h-12 bg-stone-300': hour - 1 < 8 || hour - 1 >= 17,
                            'h-32 bg-stone-100': hour - 1 >= 8 && hour - 1 < 17
                        }"> </div>
                    </template>
        </template>

        <template v-for="(item, index) in calendarItems" :key="item.id + item.type">
            <MeetingItem :item="item" :index="index" :amount="calendarItems.length" view="week" />
        </template>
    </div>
</template>

<script>
import MeetingItem from '../Components/MeetingItemDay.vue';
import { meetings } from '../Data/index.js';
import { todos } from '../Data/index.js';

export default {
    data() {
        return {
            day: 24,
            week: 7
        }
    },
    components: {
        MeetingItem
    },
    props: {
        calendarDate: Date
    },
    computed: {
        calendarItems() {
            return [...meetings, ...todos].filter(item => {
                const currentDate = new Date(this.calendarDate);
                return item.starts_at >= new Date(new Date(currentDate.setDate(currentDate.getDate() - currentDate.getDay() + 0)))
                    && item.starts_at <= new Date(new Date(currentDate.setDate(currentDate.getDate() - currentDate.getDay() + 6)))
            });
        }
    },
    methods: {
        getDayOfTheWeek: (day, locale) =>
            (new Date(0, 0, day - 1)).toLocaleDateString(locale, { weekday: 'short' }),
        getShortDate: (date, locale) =>
            date.toLocaleDateString(locale, { day: 'numeric', month: 'numeric' }),
        getDate(day) {
            const currentDate = new Date(this.calendarDate);
            return this.getShortDate(new Date(currentDate.setDate(currentDate.getDate() - currentDate.getDay() + day - 1)));
        }
    }
}
</script>