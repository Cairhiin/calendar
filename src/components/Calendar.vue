<template>
    <div class="p-8">
        <calendar-header @change-view="view => changeView(view)" @increase="view => increaseCalendarDate(view)()"
            @decrease="view => decreaseCalendarDate(view)()" :view="view" :year="year" :month="month" :day="day"
            :date="date" :week="week" :monthAsNumber="calendarDate.getMonth()" />
        <calendar-content :view="view" :calendarDate="calendarDate" />
    </div>
</template>

<script>
import CalendarContent from './Calendar/Content.vue';
import CalendarHeader from './Calendar/Header.vue';

Date.prototype.getWeekNumber = function () {
    let d = new Date(Date.UTC(this.getFullYear(), this.getMonth(), this.getDate()));
    let dayNum = d.getUTCDay() || 7;
    d.setUTCDate(d.getUTCDate() + 4 - dayNum);
    let yearStart = new Date(Date.UTC(d.getUTCFullYear(), 0, 1));
    return Math.ceil((((d - yearStart) / 86400000) + 1) / 7)
};

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
            return this.calendarDate.getFullYear();
        },
        week() {
            return this.calendarDate.getWeekNumber();
        },
    },
    methods: {
        getDayName: (date, locale) => date.toLocaleDateString(locale, { weekday: 'long' }),
        getMonthName: (date, locale) => date.toLocaleDateString(locale, { month: 'long' }),
        increaseCalendarDate(type) {
            let self = this;

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
