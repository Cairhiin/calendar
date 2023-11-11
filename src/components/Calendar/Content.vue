<template>
    <div>
        <day-view v-if="view === 'day'" />
        <week-view v-if="view === 'week'" :calendarDate="calendarDate" />
        <month-view v-if="view === 'month'" :calendarDate="calendarDate" />
        <quarter-view v-if="view === 'quarter'" :calendarDate="calendarDate" />
    </div>
</template>

<script>
import DayView from './DayView.vue';
import WeekView from './WeekView.vue';
import MonthView from './MonthView.vue';
import QuarterView from './QuarterView.vue';

export default {
    components: {
        DayView,
        WeekView,
        MonthView,
        QuarterView
    },
    props: {
        calendarDate: Date,
        view: String
    }
}
</script>