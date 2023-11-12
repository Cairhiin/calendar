<template>
    <div class="flex justify-between items-center mb-6">
        <div class="flex capitalize items-center">
            <div @click="$emit('decrease', view)"
                class="bg-slate-700 cursor-pointer w-12 text-center text-xl mr-4 text-stone-100 rounded">
                <i class="fa fa-solid fa-chevron-left"></i>
            </div>
            <div class="w-64 text-center">
                {{ `${day} ${date} ${month} ${year}` }}
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
    props: {
        year: Number,
        month: String,
        day: String,
        date: Number,
        view: String
    }
}
</script>