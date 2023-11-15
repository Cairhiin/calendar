<template>
    <div class="py-1 px-2 text-white drop-shadow" :class="{
        'bg-sky-800/75': item.type === 'meeting',
        'bg-green-800/75': item.type === 'todo',
        'bg-red-800/75': item.type === 'todo' && !isFinished
    }">
        <h4 class="truncate font-bold text-xs text-left">{{ item.title }}</h4>
    </div>
</template>
<!-- IMPLEMENT TODO ENDS_DATE INSTEAD OF STARTS_AT -->
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
