<template>
    <div class="p-8">
        <calendar-header @change-view="view => changeView(view)" @increase="view => increaseCalendarDate(view)()"
            @decrease="view => decreaseCalendarDate(view)()" :view="view" :year="year" :month="month" :day="day"
            :date="date" />
        <calendar-content :view="view" :calendarDate="calendarDate" />
    </div>
</template>

<script>
import CalendarContent from './Calendar/Content.vue';
import CalendarHeader from './Calendar/Header.vue';

export default {
    data() {
        return {
            calendarDate: new Date(),
            view: 'day'
        }
    },
    components: {
        CalendarContent,
        CalendarHeader
    },
    computed: {
        month() {
            return this.getMonthName(this.calendarDate, "nl-NL");
        },
        day() {
            return this.getDayName(this.calendarDate, "nl-NL");
        },
        date() {
            return this.calendarDate.getDate();
        },
        year() {
            return this.calendarDate.getFullYear()
        }
    },
    methods: {
        getDayName: (date, locale) => date.toLocaleDateString(locale, { weekday: 'long' }),
        getMonthName: (date, locale) => date.toLocaleDateString(locale, { month: 'long' }),
        increaseCalendarDate(type) {
            let self = this;
            console.log(self.calendarDate)
            let calendarTypes = {
                "day": function () { self.setCalendarDate(new Date(self.calendarDate.setDate(self.calendarDate.getDate() + 1))); },
                "week": function () { self.setCalendarDate(new Date(self.calendarDate.setDate(self.calendarDate.getDate() + 7))); },
                "month": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() + 1))) },
                "quarter": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() + 3))) },
                "year": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() + 12))) },
            }

            return calendarTypes[type];
        },
        decreaseCalendarDate(type) {
            let self = this;

            let calendarTypes = {
                "day": function () { self.setCalendarDate(new Date(self.calendarDate.setDate(self.calendarDate.getDate() - 1))); },
                "week": function () { self.setCalendarDate(new Date(self.calendarDate.setDate(self.calendarDate.getDate() - 7))); },
                "month": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() - 1))) },
                "quarter": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() - 3))) },
                "year": function () { self.setCalendarDate(new Date(self.calendarDate.setMonth(self.calendarDate.getMonth() - 12))) },
            }

            return calendarTypes[type];
        },
        setCalendarDate(date = new Date()) {
            this.calendarDate = date;
        },
        changeView(view) {
            this.view = view;
        }
    }
}
</script>
