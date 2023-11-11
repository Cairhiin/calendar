<template>
    <div class="grid grid-cols-3">
        <div v-for="month in quarter" class="capitalize bg-stone-200 p-2 text-center border-r border-stone-100">{{
            getQuarterMonthName(month) }}</div>
        <div class="grid grid-cols-7 py-2 px-4 text-center bg-stone-100">
            <div v-for="day in daysInFirstMonthOfQuarter" class="p-2 py-4"
                :class="{ 'border-b border-stone-200': day <= 28 }">{{ day }}
            </div>
        </div>
        <div class="grid grid-cols-7 p-2 px-4 text-center bg-stone-100">
            <div v-for="day in daysInSecondMonthOfQuarter" class="p-2 py-4"
                :class="{ 'border-b border-stone-200': day <= 28 }">{{ day }}</div>
        </div>
        <div class="grid grid-cols-7 p-2 px-4 text-center bg-stone-100">
            <div v-for="day in daysInThirdMonthOfQuarter" class="p-2 py-4"
                :class="{ 'border-b border-stone-200': day <= 28 }">{{ day }}</div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            quarter: 3,
        }
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