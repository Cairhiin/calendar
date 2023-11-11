<template>
    <div class="grid grid-cols-7">
        <div v-for="day in week" class="text-center capitalize flex-1 bg-stone-200 p-2 border-r border-stone-100">{{
            getDayOfTheWeek(day, 'nl-NL')
        }}</div>
        <div v-for="day in month" class="bg-stone-100 border-stone-200 h-32"
            :class="{ 'border-r': day % 7 !== 0, 'border-b': day <= 35 }">{{ getDate(day) }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            week: 7,
            month: 42
        }
    },
    props: {
        calendarDate: Date,
    },
    methods: {
        getDayOfTheWeek(day, locale) {

            const dayOfTheWeek = this.calendarDate.getDay();
            const startOfTheMonth = new Date(this.calendarDate.getFullYear(), this.calendarDate.getMonth());
            //start.setDate(startOfTheMonth.getDate() - dayOfTheWeek);
            //console.log(dayOfTheWeek, startOfTheMonth)
            //console.log(new Date(date.setDate(this.calendarDate.getDate() - firstDayOfMonth + day)));
            return (new Date(0, 0, day - 1)).toLocaleDateString(locale, { weekday: 'short' })
        },
        getShortDate: (date, locale) =>
            date.toLocaleDateString(locale, { day: 'numeric' }),
        getDate(day) {
            const currentDate = new Date(this.calendarDate);
            return this.getShortDate(new Date(currentDate.setDate(currentDate.getDate() - currentDate.getDay() + day - 1)));
        }
    }
}
</script>