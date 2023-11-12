<template>
    <div class="capitalize bg-stone-200 p-2 text-center border-r border-stone-100">{{
        name }}</div>
    <div class="grid grid-cols-7 py-2 px-4 text-center bg-stone-100">
        <div v-for="day in month" class="p-2 py-4" :class="{ 'border-b border-stone-200': day <= 28 }">{{ day }}
        </div>
    </div>
</template>

<script>
export default {
    props: {
        name: String,
        month: Number
    }
}
</script>