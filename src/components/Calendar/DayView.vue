<template>
    <div>
        <div v-for="hour in day" class="flex items-start" :class="{ 'items-center': hour < 8 || hour >= 17 }">
            <div class="pr-2 text-right">
                {{ hour < 10 ? `0${hour}` : hour }}:00 </div>
                    <div :class="{
                        'h-32': hour >= 8 && hour < 17,
                        'h-12 bg-stone-300': hour < 8 || hour >= 17
                    }" class="border-b border-stone-200 bg-stone-100 flex-1 px-4"></div>
            </div>
        </div>
</template>

<script>
export default {
    data() {
        return {
            day: 24
        }
    }
}
</script>