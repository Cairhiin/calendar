<template>
    <div class="grid grid-cols-1 lg:grid-cols-3 xl:grid-cols-4 bg-stone-100">
        <month-table v-for="month in year">
            <div class="capitalize bg-stone-200 p-2 text-center border-r border-stone-100">{{
                getMonthName(getMonthOfYear(month), 'nl-NL') }}</div>
            <div class="grid grid-cols-7 py-2 px-4 text-center bg-stone-100">
                <div v-for="day in daysInMonth[month - 1]" class="p-2 py-4"
                    :class="{ 'border-b border-stone-200': day <= 28 && month !== 2 || day <= 21 && month === 2 }">
                    {{ day }}
                </div>
            </div>
        </month-table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            year: 12
        }
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