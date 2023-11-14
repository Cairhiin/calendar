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
            <template v-for="(item, index) in calendarItems" :key="calendarItems.id">
                <MeetingItem :item="item" :index="index" :amount="calendarItems.length" />
            </template>
        </div>
</template>

<script>
import MeetingItem from '../Components/MeetingItem.vue';

export default {
    data() {
        return {
            day: 24
        }
    },
    components: {
        MeetingItem
    },
    props: {
        calendarItems: Object
    },
}
</script>