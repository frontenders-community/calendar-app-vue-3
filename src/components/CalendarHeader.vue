<script lang="ts" setup>
import { modes } from '@/models/calendar';
import { onMounted } from 'vue';

defineEmits(['modeChange', 'showNext', 'showPrev']);

const props = defineProps({
    selectedMode: String,
    title: String
});


const modesArray = Object.keys(modes).map(key => modes[key]);

</script>

<template>
    <div class="calendar-menu">
        <nav class="flex align-items-center justify-content-between">
            <ul class="mode-menu">
                <li v-for="mode in modesArray" :key="mode" :class="{ 'active': mode === selectedMode }"
                    @click="$emit('modeChange', mode)">
                    {{ mode }}
                </li>
            </ul>

            <div class="date-menu">
                <a id="prev" href="" @click.prevent="$emit('showPrev')">
                    <i class="fa-solid fa-chevron-left"></i>
                </a>
                <a id="current" href="">
                    {{ props.title }}
                </a>
                <a id="next" href="" @click.prevent="$emit('showNext')">
                    <i class="fa-solid fa-chevron-right"></i>
                </a>
            </div>

            <div>
                <button class="add-btn">Aggiungi un evento</button>
            </div>
        </nav>
    </div>
</template>

<style lang="scss" scoped>
@use '../styles/partials/variables' as *;


.calendar-menu {
    a {
        text-decoration: none;
        color: $text-light-color;

        &#current {
            color: $text-color;
            font-weight: 500;
            margin: 0 .5rem;
        }
    }

    .mode-menu {
        list-style-type: none;
        display: flex;
        gap: 5px;

        li {
            font-size: 1rem;
            font-weight: 300;
            cursor: pointer;
            padding: .2rem .5rem;
            border-radius: 5px;

            &.active {
                color: $secondary-color;
                background-color: lighten($color: $secondary-color, $amount: 30%);
            }
        }
    }

    .add-btn {
        background-color: $primary-color;
        padding: .6rem .8rem;
        color: white;
        font-weight: 500;
        font-size: .8rem;
        border: 0;
        border-radius: 5px;
        cursor: pointer;
        transition: all .1s;

        &:hover {
            background-color: darken($color: $primary-color, $amount: 5%);
        }
    }
}
</style>