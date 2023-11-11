<template>
    <div class="uppercase text-xs rounded">
        <span @click="$parent.$emit('changeView', 'day')"
            class="font-bold border-r-2 border-white p-4 rounded-l bg-slate-100 cursor-pointer"
            :class="{ 'bg-slate-700 text-white': activeView === 'day' }">Dag</span>
        <span @click="$parent.$emit('changeView', 'week')" class="bg-slate-100 cursor-pointer border-r-2 border-white p-4"
            :class="{ 'bg-slate-700 text-white': activeView === 'week' }">Week</span>
        <span @click="$parent.$emit('changeView', 'month')" class="bg-slate-100 cursor-pointer border-r-2 border-white p-4"
            :class="{ 'bg-slate-700 text-white': activeView === 'month' }">Maand</span>
        <span @click="$parent.$emit('changeView', 'quarter')"
            class="bg-slate-100 cursor-pointer border-r-2 border-white p-4"
            :class="{ 'bg-slate-700 text-white': activeView === 'quarter' }">Kwartaal</span>
        <span @click="$parent.$emit('changeView', 'year')"
            class="bg-slate-100 cursor-pointer border-r-2 border-white p-4 rounded-r"
            :class="{ 'bg-slate-700 text-white': activeView === 'year' }">Jaar</span>
    </div>
</template>

<script>
export default {
    props: {
        activeView: String
    }
}
</script>