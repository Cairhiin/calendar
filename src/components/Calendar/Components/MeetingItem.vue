<template>
    <div class="bg-green-700" :class="{
        'bg-sky-700': type === 'meeting',
        'bg-red-700': type === 'todo' && !isFinished
    }">
        <h4 class="text-white font-bold text-sm">data.title</h4>
    </div>
</template>

<script>
export default {
    computed: {
        isFinished() {
            return !(new Date(this.todo.due_date) > new Date() && !this.todo.data_completed)
        }
    },
    props: {
        type: String,
        data: Object
    }
}
</script>