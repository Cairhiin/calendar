<template>
    <div class="grid grid-cols-1 grid-rows-1 lg:grid-cols-3 xl:grid-cols-4">
        <month-table v-for="month in year" :name="getMonthName(getMonthOfYear(month), 'nl-NL')"
            :month="daysInMonth[month - 1]" class="mr-[1px]" :key="month" />
    </div>
</template>

<script>
import MonthTable from '../Components/MonthTable.vue';

export default {
    data() {
        return {
            year: 12
        }
    },
    components: {
        MonthTable
    },
    props: {
        calendarDate: Date
    },
    computed: {
        daysInMonth() {
            return Array.from(Array(12).keys()).map(n => this.getDaysInMonth(n));
        }
    },
    methods: {
        getMonthOfYear(date) {
            const currentDate = new Date(this.calendarDate);
            return new Date(currentDate.setMonth(date - 1));
        },
        getDaysInMonth(month) {
            return new Date(this.calendarDate.getFullYear(), month + 1, 0).getDate();
        },
        getMonthName: (date, locale) => date.toLocaleDateString(locale, { month: 'long' }),
    }
}
</script>