<template>
    <button :class="__class" class="flex justify-center" style="white-space: nowrap" :disabled="disabled">
        <span class="my-auto">
            <slot></slot>
        </span>
        <component v-if="icon || loading" :class="`${!iconOnly ? 'mr-2' : ''} my-auto`" size="30" :color="types[type]"
            :is="loading ? Spinner : icon" class="icon"></component>

    </button>
    <!-- <spinner></spinner> -->
</template>

<script lang="ts" setup>
import Spinner from '../svgs/loader-icon.vue'


import { computed } from "@vue/reactivity"

let props = defineProps({
    size: {
        type: String,
        default: 'medium'
    },
    type: {
        type: String,
        default: 'pry'
    },
    round: {
        type: Boolean,
        default: false
    },
    icon: {
        type: Object
    },
    iconOnly: {
        type: Boolean,
        defult: false
    },
    full: {
        type: Boolean,
        default: false
    },
    disabled: {
        type: Boolean,
        default: false
    },
    loading: {
        type: Boolean,
        default: false
    }
})

let base = 'base'
let sizes = {
    small: 'small ',
    medium: 'medium',
    big: ''
}
let types = {
    'pry': {
        __class: ' btn-grad',
        text: '#03053D'
    },
}

let __class = computed(() => {
    let __base = ((types[props.type] && types[props.type].__class) || types['pry'].__class) + ' ' + (sizes[props.size] || sizes['medium']);
    if (props.round)
        __base += ' rounded-full'
    else
        __base += ' ' + base;

    if (props.full)
        __base += ' w-full'
    if (props.disabled)
        __base += ' fade-40'

    return __base
})

</script>

<style scoped>
button {
    cursor: pointer;
    display: flex;
    justify-content: center;
    white-space: nowrap;
}

.icon {
    margin-left: 5px;
}

.rounded-full {
    border-radius: 50%;
}

.w-full {
    width: 100%;
}

.fade-40 {
    opacity: 0.4;
}

.small {
    padding: 5px 10px;
    font-size: 1.2rem;
    text-align: center;
}

.medium {
    padding: 10px 20px;
    font-size: 1.2rem;
    text-align: center;
}

.my-auto {
    margin-top: auto;
    margin-bottom: auto;
}
</style>