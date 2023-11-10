<template>
    <div>
        <day-view v-if="view === 'day'" />
        <week-view v-if="view === 'week'" :calendarDate="calendarDate" />
    </div>
</template>
<script>
import DayView from './DayView.vue';
import WeekView
    from './WeekView.vue';
export default {
    components: {
        DayView,
        WeekView
    },
    props: {
        calendarDate: Date,
        view: String
    }
}
</script>