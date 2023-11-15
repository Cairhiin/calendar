<template>
    <div class="py-1 px-2 text-white/75 bg-sky-800/75 drop-shadow" :class="{
        'bg-green-800/75': item.type === 'todo',
        'bg-red-800/75': item.type === 'todo' && !isFinished
    }">
    </div>
</template>
<script>
export default {
    props: {
        item: Object
    },
    computed: {
        isFinished() {
            return new Date(this.item.ends_at) > new Date() && !this.item.data_completed;
        },
    }
}
</script>