<template>
    <div class="g-score">
        <span class="g-score__value">{{ modelValue }}</span>
        <GIconHeart />
    </div>
</template>

<script setup>
import GIconHeart from './icons/GIconHeart.vue';

defineProps({
    modelValue: {
        type: Number,
        default: 0
    }
})
</script>

<style scoped>
.g-score {
    background-color: var(--color-bg-blue);
    border-radius: 2.25rem;
    padding: .75rem 1.5rem;
    box-sizing: border-box;
    color: var(--color-primary);
    display: flex;
    align-items: center;
    gap: .625rem;
    box-shadow: 0px 0px 4px 0px #00000026;
}

.g-score__value {
    font-weight: 700;
    font-size: 1rem;
    line-height: 150%;
}
</style>