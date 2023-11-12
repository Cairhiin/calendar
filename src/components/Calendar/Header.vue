<template>
    <div class="flex justify-between items-center mb-6">
        <div class="flex capitalize items-center">
            <div @click="$emit('decrease', view)"
                class="bg-slate-700 cursor-pointer w-12 text-center text-xl mr-4 text-stone-100 rounded">
                <i class="fa fa-solid fa-chevron-left"></i>
            </div>
            <div class="w-64 text-center">
                {{ headerTitle }}
            </div>
            <div @click="$emit('increase', view)"
                class="bg-slate-700 cursor-pointer w-12 text-center text-xl ml-4 text-stone-100 rounded">
                <i class="fa fa-solid fa-chevron-right"></i>
            </div>
        </div>
        <view-switch-bar :activeView="view" />
    </div>
</template>
<script>
import ViewSwitchBar from './Components/ViewSwitchBar.vue';

export default {
    components: {
        ViewSwitchBar
    },
    computed: {
        headerTitle() {
            if (this.view === "year") return this.year;
            if (this.view === "day") return `${this.day} ${this.date} ${this.month} ${this.year}`;
            if (this.view === "week") return `Week ${this.week} - ${this.year} `;
            if (this.view === "month") return `${this.month} ${this.year}`;
            if (this.view === "quarter") return `Kwartaal ${Math.ceil((this.monthAsNumber + 1) / 3)} - ${this.year}`
        }
    },
    props: {
        year: Number,
        month: String,
        day: String,
        week: Number,
        date: Number,
        monthAsNumber: Number,
        view: String
    }
}
</script>