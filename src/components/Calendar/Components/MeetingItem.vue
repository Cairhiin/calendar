<template>
    <div v-if="view === 'day'" class="bg-green-800/75 absolute p-2 text-white drop-shadow overflow-hidden" :style="{
        'height': `${height}px`,
        'top': `${positionY}px`,
        'left': `50px`,
        'width': `calc(100% - 50px)`
    }" :class="{
    'bg-sky-800/75': item.type === 'meeting',
    'bg-red-800/75': item.type === 'todo' && !isFinished,
}">

        <h4 class="font-bold text-sm uppercase">{{ item.title }}</h4>
        <p class=" text-slate-200/75 text-sm lg:text-base">{{ item.description }}</p>
        <div class="flex gap-4 mt-2">
            <p class="text-slate-200/75 text-sm lg:text-base"><span class="text-white">Starttijd:</span> {{
                item.starts_at.toLocaleString('nl-NL') }}
            </p>
            <p class=" text-slate-200/75 text-sm lg:text-base"><span class="text-white">Eindtijd:</span> {{
                item.ends_at.toLocaleString('nl-NL') }}
            </p>
        </div>
    </div>
    <div v-else class="bg-green-800/75 absolute p-2 text-white drop-shadow overflow-hidden" :style="{
        'height': `${height}px`,
        'top': `${positionY}px`,
        'left': `calc(${dayOfTheWeek} * 100%  / 7)`,
        'width': `calc(100%  / 7 )`
    }" :class="{
    'bg-sky-800/75 z-10': item.type === 'meeting',
    'bg-red-800/75': item.type === 'todo' && !isFinished,
}">

        <h4 class="truncate xl:whitespace-normal font-bold text-xs xl:text-sm uppercase">{{ item.title }}</h4>
        <p class="truncate xl:whitespace-normal text-slate-200/75 text-xs xl:text-sm">{{ item.description }}</p>
    </div>
</template>

<script>
const WORK_HOUR_CELL_HEIGHT = 127;
const NONWORK_HOUR_CELL_HEIGHT = 47;
const BORDER = 1;
const WORK_DAY_START = 8;
const WORK_DAY_END = 17;

export default {
    computed: {
        dayOfTheWeek() {
            if (this.item.type === 'meeting') {
                return new Date(this.item.starts_at).getDay();
            }

            return new Date(this.item.ends_at).getDay();
        },
        isFinished() {
            return new Date(this.item.ends_at) > new Date() && !this.item.data_completed;
        },
        height() {
            const beginsBeforeWorkingHours = this.item.starts_at.getHours() < WORK_DAY_START;
            const endsBeforeWorkingHours = this.item.ends_at.getHours() < WORK_DAY_START;
            const beginsAfterWorkingHours = this.item.starts_at.getHours() >= WORK_DAY_END;
            const endsAfterWorkingHours = this.item.ends_at.getHours() >= WORK_DAY_END;

            if (this.item.type === 'todo') {
                return (WORK_HOUR_CELL_HEIGHT + BORDER);
            }

            if (this.item.ends_at.getDate() !== this.item.starts_at.getDate()) {
                return (WORK_DAY_END - this.item.starts_at.getHours()) * (WORK_HOUR_CELL_HEIGHT + BORDER) + 7 * (NONWORK_HOUR_CELL_HEIGHT + BORDER);
            }

            if (beginsBeforeWorkingHours) {
                const beforeWorkingDay = endsBeforeWorkingHours ?
                    ((this.item.ends_at.getHours() + this.item.ends_at.getMinutes() / 60)
                        - (this.item.starts_at.getHours() + this.item.starts_at.getMinutes() / 60))
                    * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                    : (WORK_DAY_START - this.item.starts_at.getHours() - this.item.starts_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER);
                const duringWorkingDay =
                    !endsBeforeWorkingHours ?
                        endsAfterWorkingHours ?
                            (WORK_DAY_END - WORK_DAY_START) * (WORK_HOUR_CELL_HEIGHT + BORDER)
                            + (this.item.ends_at.getHours() - WORK_DAY_END + this.item.ends_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER) :
                            (this.item.ends_at.getHours() - WORK_DAY_START + this.item.ends_at.getMinutes() / 60) * (WORK_HOUR_CELL_HEIGHT + BORDER)
                        : 0;
                return (
                    beforeWorkingDay +
                    duringWorkingDay
                );
            }

            if (beginsAfterWorkingHours) {
                return ((this.item.ends_at.getHours() + this.item.ends_at.getMinutes() / 60)
                    - (this.item.starts_at.getHours() + this.item.starts_at.getMinutes() / 60))
                    * (NONWORK_HOUR_CELL_HEIGHT + BORDER);
            }

            if (endsAfterWorkingHours) {
                return (
                    (WORK_DAY_END - this.item.starts_at.getHours() - this.item.starts_at.getMinutes() / 60) * (WORK_HOUR_CELL_HEIGHT + BORDER)
                    + (this.item.ends_at.getHours() - WORK_DAY_END + this.item.ends_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                );
            }

            return Math.round((this.item.ends_at.getTime() - this.item.starts_at.getTime()) / 60000) / 60 * (WORK_HOUR_CELL_HEIGHT + BORDER);
        },
        positionY() {
            if (this.item.type === 'todo') {
                if (this.item.ends_at.getHours() >= WORK_DAY_START && this.item.ends_at.getHours() < WORK_DAY_END) {
                    return 8 * 48 + (this.item.ends_at.getHours() - 9 + this.item.ends_at.getMinutes() / 60) * 128;
                }

                if (this.item.ends_at.getHours() >= WORK_DAY_END) {
                    return (
                        WORK_DAY_START * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                        + (WORK_DAY_END - WORK_DAY_START) * (WORK_HOUR_CELL_HEIGHT + BORDER)
                        + (this.item.ends_at.getHours() - 1 - WORK_DAY_END + this.item.ends_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                    );
                }

                return (this.item.ends_at.getHours() - 1 + this.item.ends_at.getMinutes() / 60) * 48;
            }

            if (this.item.starts_at.getHours() >= WORK_DAY_START && this.item.starts_at.getHours() < WORK_DAY_END) {
                return 8 * 48 + (this.item.starts_at.getHours() - 8 + this.item.starts_at.getMinutes() / 60) * 128;
            }

            if (this.item.starts_at.getHours() >= WORK_DAY_END) {
                return (
                    WORK_DAY_START * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                    + (WORK_DAY_END - WORK_DAY_START) * (WORK_HOUR_CELL_HEIGHT + BORDER)
                    + (this.item.starts_at.getHours() - WORK_DAY_END + this.item.starts_at.getMinutes() / 60) * (NONWORK_HOUR_CELL_HEIGHT + BORDER)
                );
            }

            return (this.item.starts_at.getHours() + this.item.starts_at.getMinutes() / 60) * 48;
        },
    },
    props: {
        view: {
            type: String,
            default: 'day'
        },
        item: Object,
        amount: Number,
        index: Number
    }
}
</script>