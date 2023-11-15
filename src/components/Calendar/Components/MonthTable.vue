<template>
    <div class="bg-stone-100">
        <div class="capitalize bg-stone-200 p-2 text-center border-r">{{
            name }}</div>
        <div class="grid grid-cols-7 py-2 px-4 text-center bg-stone-100">
            <div v-for="day in month" class="h-16 w-full relative grid grid-rows-1 p-1"
                :class="{ 'border-b border-stone-200': day <= 28 }">
                <span class="absolute left-1/4 right-1/4 top-5 font-bold z-10"
                    :class="{ 'text-stone-100': getIsCalendarEventToday(day), 'text-stone-600': !getIsCalendarEventToday(day) }">{{
                        day }}</span>
                <template v-for="item in calendarItems" :key="item.id + item.type">
                    <MeetingItem v-if="getIsCalendarItemToday(item, day)" :item="item" />
                </template>
            </div>
        </div>
    </div>
</template>

<script>
import MeetingItem from './MeetingItemSmall.vue';

export default {
    components: {
        MeetingItem
    },
    methods: {
        getIsCalendarItemToday(item, day) {
            if (item.type === 'meeting') {
                return item.starts_at.getDate() === day;
            }

            return item.ends_at.getDate() === day;
        },
        getIsCalendarEventToday(day) {
            return this.calendarItems.filter(item => {
                if (item.type === 'meeting') {
                    return item.starts_at.getDate() === day;
                }

                return item.ends_at.getDate() === day;
            }).length > 0;
        }
    },
    props: {
        name: String,
        month: Number,
        calendarItems: Object,
        calendarDate: Date
    }
}
</script>