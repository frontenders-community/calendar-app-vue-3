<script lang="ts" setup>
import CalendarHeader from './CalendarHeader.vue';
import { DateTime, Info } from 'luxon';
import { computed, onMounted, reactive } from 'vue';
import { modes } from '../models/calendar';

type State = {
    selectedMode: string,
    datesRange: Array<DateTime>,
    selectedDate: DateTime | null,
    curDate: DateTime | null,
}

const weekdays = Info.weekdays('short');

const state: State = reactive({
    selectedMode: modes.MONTH,
    selectedDate: null,
    datesRange: [],
    curDate: null,
});


onMounted(() => {
    state.curDate = DateTime.now();
    state.selectedDate = state.curDate;
    generateRange();
})

const calendarTitle = computed(() => {
    if (!state.selectedDate) return;
    if (state.selectedMode === modes.MONTH) {
        return state.selectedDate.toLocaleString({ month: 'short', year: 'numeric' });
    } else {
        return state.selectedDate.toLocaleString({ day: 'numeric', month: 'short', year: 'numeric' });
    }
})

const gridTemplateRows = computed(() => {
    const rowsNum = state.datesRange.length % 7;
    let templateRows = "";
    for (let i = 0; i < rowsNum; i++) {
        templateRows += "1fr ";
    }
    return templateRows;
})

const gridTemplateColumns = computed(() => {
    if (state.selectedMode === modes.DAY) {
        return '1fr';
    } else {
        return '1fr 1fr 1fr 1fr 1fr 1fr 1fr'
    }
})

function isInSelectedRange(date: DateTime) {
    if (!state.selectedDate) return;
    if (state.selectedMode === modes.MONTH) {
        return state.selectedDate.month === date.month;
    } else {
        return true;
    }
}

function isCurrent(date: DateTime) {
    return date.hasSame(state.curDate, 'day');
}

function generateRange() {
    let startDate = state.selectedDate.startOf(state.selectedMode);
    let endDate = state.selectedDate.endOf(state.selectedMode);

    if (state.selectedMode !== modes.DAY) {
        if (startDate.toLocaleString({ weekday: 'short' }) !== weekdays[0]) {
            startDate = startDate.startOf('week');
        }

        if (endDate.toLocaleString({ weekday: 'short' }) !== weekdays[weekdays.length - 1]) {
            endDate = endDate.endOf('week');
        }
    }

    let limit = Math.floor(endDate.diff(startDate, 'days').days);
    state.datesRange = [];
    for (let i = 0; i <= limit; i++) {
        state.datesRange.push(startDate.plus({ days: i }));
    }
}


function handleModeChange(newMode: string) {
    state.selectedMode = newMode;
    if (newMode === modes.WEEK && state.selectedDate.toLocaleString({ weekday: 'short' }) !== weekdays[0]) {
        state.selectedDate = state.selectedDate.startOf('week')
    }
    generateRange();
}

function handleShowNext() {
    if (!state.selectedMode) return;
    const toAdd = state.selectedMode + 's';
    state.selectedDate = state.selectedDate.plus({ [toAdd]: 1 });
    generateRange();
}

function handleShowPrev() {
    if (!state.selectedMode) return;
    const toAdd = state.selectedMode + 's';
    state.selectedDate = state.selectedDate.minus({ [toAdd]: 1 });
    generateRange();
}

</script>

<template>
    <div class="inner-wrapper container">
        <CalendarHeader :title="calendarTitle" :selectedMode="state.selectedMode" @modeChange="handleModeChange"
            @showNext="handleShowNext" @showPrev="handleShowPrev" />

        <div class="calendar">
            <div class="calendar-heading" v-if="state.selectedMode !== modes.DAY">
                <div class="col" v-for="weekday in weekdays" :key="weekday">{{ weekday }}</div>
            </div>
            <div class="calendar-body"
                :style="{ 'grid-template-rows': gridTemplateRows, 'grid-template-columns': gridTemplateColumns }">
                <div class="cell" :class="{ 'in-selected': isInSelectedRange(date), 'current': isCurrent(date) }"
                    v-for="date in state.datesRange">
                    <h5 class="date">{{ date.day }}</h5>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@use '../styles/partials/variables' as *;

.inner-wrapper {
    height: 100%;
    display: flex;
    flex-direction: column;
}

.calendar {
    flex-grow: 1;
    padding: 1rem;
    display: flex;
    flex-direction: column;

    .calendar-heading {
        padding: 1rem 0;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
        width: 100%;
        gap: 1rem;
        text-align: right;
        color: $text-light-color;
    }

    .calendar-body {
        flex-grow: 1;
        display: grid;
        gap: 1rem;

        .cell {
            background-color: white;
            padding: .5rem;
            border-radius: 5px;

            &.in-selected {
                .date {
                    color: $text-color;
                }
            }

            &.current {
                border: 1px solid lighten($secondary-color, 30%);

                .date {
                    color: $secondary-color;
                }
            }

            .date {
                color: lighten($text-color, 60%);
                text-align: end;
                font-size: .8rem;
                font-weight: 300;
            }
        }
    }
}
</style>