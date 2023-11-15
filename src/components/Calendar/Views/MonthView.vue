<template>
    <div class="grid grid-cols-7">
        <div v-for="day in week" :key="day"
            class="text-center capitalize flex-1 bg-stone-200 p-2 border-r border-stone-100">{{
                getDayOfTheWeek(day, 'nl-NL')
            }}</div>
        <div v-for="day in month" class="bg-stone-100 border-stone-200 h-32 font-bold text-right relative" :class="{
            'border-r': day % 7 !== 0,
            'border-b': day <= 35,
            'text-stone-400 bg-stone-300': getMonth(day) !== calendarDate.getMonth(),
            'text-stone-900': getMonth(day) === calendarDate.getMonth()
        }"><span class="absolute bottom-1 right-2 text-xl">{{ getDate(day) }}</span>
            <div class="grid grid-rows-5 h-full grid-cols-1">
                <template v-for="(item, index) in getCalendarItems(day)" :key="item.id + item.type">
                    <MeetingItem v-if="getMonth(day) === calendarDate.getMonth()" :item="item" />
                </template>
            </div>
        </div>
    </div>
</template>

<script>
import MeetingItem from '../Components/MeetingItemMonth.vue';
import { meetings } from '../Data/index.js';
import { todos } from '../Data/index.js';

export default {
    data() {
        return {
            week: 7,
            month: 42
        }
    },
    props: {
        calendarDate: Date,
    },
    computed: {
        calendarItems() {
            return [...meetings, ...todos].filter(item => {
                const currentDate = new Date(this.calendarDate);
                return item.starts_at >= new Date(new Date(currentDate.getFullYear(), currentDate.getMonth(), 1))
                    && item.starts_at <= new Date(new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0))
            });
        }
    },
    components: {
        MeetingItem
    },
    methods: {
        getCalendarItems(day) {
            const today = new Date(this.calendarDate.getFullYear(), this.calendarDate.getMonth(), this.getDate(day));
            return this.calendarItems.filter(item => item.starts_at.getDate() === today.getDate());
        },
        getDayOfTheWeek(day, locale) {
            return (new Date(0, 0, day - 1)).toLocaleDateString(locale, { weekday: 'short' })
        },
        getShortDate: (date, locale) =>
            date.toLocaleDateString(locale, { day: 'numeric' }),
        getDate(day) {
            const currentDate = new Date(this.calendarDate);
            const startOfTheMonth = new Date(this.calendarDate.getFullYear(), this.calendarDate.getMonth()).getDay();
            return this.getShortDate(new Date(currentDate.setDate(1 - startOfTheMonth + day - 1)));
        },
        getMonth(day) {
            const currentDate = new Date(this.calendarDate);
            const startOfTheMonth = new Date(this.calendarDate.getFullYear(), this.calendarDate.getMonth()).getDay();
            return new Date(currentDate.setDate(1 - startOfTheMonth + day - 1)).getMonth();
        }
    }
}
</script>