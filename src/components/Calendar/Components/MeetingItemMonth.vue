<template>
    <div class="py-1 px-2 text-white/75 bg-sky-800/75 drop-shadow" :class="{
        'bg-green-800/75': item.type === 'todo',
        'bg-red-800/75': item.type === 'todo' && !isFinished
    }">
        <h4 class="truncate font-bold text-xs text-left">{{ item.title }}</h4>
    </div>
</template>
<script>
export default {
    props: {
        item: Object
    }
}
</script>