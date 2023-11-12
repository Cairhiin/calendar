<template>
    <div class="grid grid-cols-1 lg:grid-cols-3">
        <month-table v-for="month in quarter" :name="getQuarterMonthName(month)" :month="daysInQuarterMonth(month)"
            class="mr-[1px]" />
    </div>
</template>
<script>
import MonthTable from '../Components/MonthTable.vue';

export default {
    data() {
        return {
            quarter: 3,
        }
    },
    components: {
        MonthTable
    },
    props: {
        calendarDate: Date
    },
    computed: {
        daysInFirstMonthOfQuarter() {
            return this.getDaysInMonth(0);
        },
        daysInSecondMonthOfQuarter() {
            return this.getDaysInMonth(1);
        },
        daysInThirdMonthOfQuarter() {
            return this.getDaysInMonth(2);
        },
    },
    methods: {
        daysInQuarterMonth(month) {
            if (month === 1) return this.daysInFirstMonthOfQuarter;
            if (month === 2) return this.daysInSecondMonthOfQuarter;
            return this.daysInThirdMonthOfQuarter;
        },
        getMonthOfQuarter(month) {
            const currentDate = new Date(this.calendarDate);
            return currentDate.setMonth(this.calendarDate.getMonth() + month - (this.calendarDate.getMonth() % 3) - 1);
        },
        getQuarterMonthName(month) {
            return this.getMonthName(new Date(this.getMonthOfQuarter(month)), 'nl-NL');
        },
        getMonthName: (date, locale) => date.toLocaleDateString(locale, { month: 'long' }),
        getDaysInMonth(month) {
            const currentMonth = new Date(this.getMonthOfQuarter(month)).getMonth();
            return new Date(this.calendarDate.getFullYear(), currentMonth, 0).getDate();
        }
    }

}
</script>