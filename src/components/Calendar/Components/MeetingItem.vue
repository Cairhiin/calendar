<template>
    <div class="bg-green-800/75 absolute rounded p-2 text-white shadow" :style="{
        'height': `${height}px`,
        'top': `${positionY}px`,
        'left': '50px',
        'width': `calc(${100}% - 50px)`
    }" :class="{
    'bg-sky-800/75': item.type === 'meeting',
    'bg-red-800/50': item.type === 'todo' && !isFinished,
}">
        <h4 class="font-bold text-sm uppercase">{{ item.title }}</h4>
        <p class="truncate text-slate-200/75">{{ item.description }}</p>
    </div>
</template>

<script>
const WORK_HOUR_CELL_HEIGHT = 127;
const NONWORK_HOUR_CELL_HEIGHT = 47;
const BORDER = 1;

export default {
    data() {
        return {

        }
    },
    computed: {
        isFinished() {
            return new Date(this.item.ends_at) > new Date() && !this.item.data_completed;
        },
        height() {
            const isBeforeWorkingHours = this.item.starts_at.getHours() < 8;
            const endsBeforeWorkingHours = this.item.ends_at.getHours() < 8;

            if (this.item.ends_at.getDate() !== this.item.starts_at.getDate()) {
                return (17 - this.item.starts_at.getHours()) * (WORK_HOUR_CELL_HEIGHT + BORDER) + 7 * (NONWORK_HOUR_CELL_HEIGHT + BORDER);
            }

            if (isBeforeWorkingHours) {
                const beforeWorkingDay = endsBeforeWorkingHours ?
                    ((this.item.ends_at.getHours() + this.item.ends_at.getMinutes() / 60)
                        - (this.item.starts_at.getHours() + this.item.starts_at.getMinutes() / 60))
                    * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                    : (8 - this.item.starts_at.getHours() - this.item.starts_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER);
                const endAmount = !endsBeforeWorkingHours ?
                    (this.item.ends_at.getHours() - 8 + this.item.ends_at.getMinutes() / 60) * (WORK_HOUR_CELL_HEIGHT + BORDER) : 0;
                return (
                    beforeWorkingDay +
                    endAmount
                );
            }

            //return Math.round((this.item.ends_at.getTime() - this.item.starts_at.getTime()) / 60000) / 60 * 127;

        },
        width() {
            return
        },
        positionY() {
            return this.calculateY();
        },
    },
    methods: {
        calculateY() {
            if (this.item.starts_at.getHours() >= 8) return 8 * 48 + (this.item.starts_at.getHours() - 8 + this.item.starts_at.getMinutes() / 60) * 128;
            else return (this.item.starts_at.getHours() + this.item.starts_at.getMinutes() / 60) * 48;
        }
    },
    props: {
        type: String,
        item: Object,
    }
}
</script>