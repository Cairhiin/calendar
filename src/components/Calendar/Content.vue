<template>
    <div>
        <day-view v-if="view === 'day'" :calendarItems="calendarItems" />
        <week-view v-if="view === 'week'" :calendarDate="calendarDate" />
        <month-view v-if="view === 'month'" :calendarDate="calendarDate" />
        <quarter-view v-if="view === 'quarter'" :calendarDate="calendarDate" />
        <year-view v-if="view === 'year'" :calendarDate="calendarDate" />
    </div>
</template>

<script>
import DayView from './Views/DayView.vue';
import WeekView from './Views/WeekView.vue';
import MonthView from './Views/MonthView.vue';
import QuarterView from './Views/QuarterView.vue';
import YearView from './Views/YearView.vue';
import { meetings } from './Data/index.js';
import { todos } from './Data/index.js';

export default {
    components: {
        DayView,
        WeekView,
        MonthView,
        QuarterView,
        YearView
    },
    computed: {
        calendarItems() {
            return [...meetings, ...todos].filter(item => new Date(item.starts_at).getDate() === this.calendarDate.getDate());
        }
    },
    props: {
        calendarDate: Date,
        view: String
    }
}
</script>