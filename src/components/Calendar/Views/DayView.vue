<template>
    <div class="relative">
        <div v-for="hour in day" class="flex items-start" :key="hour">
            <div class="pr-2 text-right -mt-3">
                {{ hour - 1 < 10 ? `0${hour - 1}` : hour - 1 }}:00 </div>
                    <div :class="{
                        'h-32': hour - 1 >= 8 && hour - 1 < 17,
                        'h-12 bg-stone-300': hour - 1 < 8 || hour - 1 >= 17
                    }" class="border-b border-stone-200 bg-stone-100 flex-1">
                    </div>
            </div>
            <template v-for="(item, index) in calendarItems" :key="item.id + item.type">
                <MeetingItem :item="item" :index="index" :amount="calendarItems.length" />
            </template>
        </div>
</template>

<script>
import MeetingItem from '../Components/MeetingItem.vue';
import { meetings } from '../Data/index.js';
import { todos } from '../Data/index.js';

export default {
    data() {
        return {
            day: 24
        }
    },
    computed: {
        calendarItems() {
            return [
                ...meetings.filter(item => new Date(item.starts_at).getDate() === this.calendarDate.getDate()),
                ...todos.filter(item => new Date(item.ends_at).getDate() === this.calendarDate.getDate())];
        }
    },
    components: {
        MeetingItem
    },
    props: {
        calendarDate: Date
    }
}
</script>