<template>
    <div class="g-card" :class="{ 'g-card--flipped': !isFlipped }">
        <div class="g-card-container">
            <!-- Лицевая сторона -->
            <div class="g-card-frontface">
                <div class="g-card-wrapper">
                    <div class="g-card__order">{{ number }}</div>
                    <div class="g-card__title">{{ titleData.ru }}</div>

                    <div class="g-card-footer">
                        <button type="button" class="g-card-footer__flip-button"
                            @click="emit('flip')">Перевернуть</button>
                    </div>
                </div>
            </div>

            <!-- Обратная сторона -->
            <div class="g-card-backface">
                <div class="g-card-wrapper">
                    <div class="g-card__order">{{ number }}</div>
                    <div class="g-card__title">{{ titleData.en }}</div>

                    <div class="g-card-footer">
                        <button type="button" class="g-card-footer__action-button" @click="emit('accept')">
                            <GIconAccept size="24" />
                        </button>
                        <button type="button" class="g-card-footer__action-button" @click="emit('reject')">
                            <GIconReject size="24" />
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import GIconReject from './icons/GIconReject.vue';
import GIconAccept from './icons/GIconAccept.vue';

const emit = defineEmits(['flip']);

const props = defineProps({
    number: {
        type: Number,
        default: 1
    },
    titleData: {
        type: Object,
        default: () => ({
            ru: 'Заголовок',
            en: 'Title'
        })
    }
})

const isFlipped = ref(false);

const number = computed(() => {
    return props.number < 10 ? `0${props.number}` : props.number;
})
</script>

<style scoped>
.g-card {
    width: 250px;
    height: 376px;
    perspective: 1000px;
    box-sizing: border-box;
}

.g-card-container {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: stretch;
    justify-content: center;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.6s;
    background-color: var(--color-bg-white);
    box-shadow: 0px 0px 16px 0px #0000001A;
    border-radius: 1rem;
}

.g-card-wrapper {
    width: 100%;
    height: 100%;
    border: 1px solid var(--color-border);
    border-radius: 0.75rem;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1;
    background-color: transparent;
    position: relative;
}

.g-card-frontface,
.g-card-backface {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    padding: 1.75rem 1.1875rem;
    box-sizing: border-box;
}

.g-card-backface {
    transform: rotateY(180deg);
}

.g-card__order {
    position: absolute;
    top: -0.5rem;
    left: 1rem;
    font-size: .875rem;
    font-weight: 400;
    color: var(--color-text-primary);
    background-color: var(--color-bg-white);
    z-index: 2;
}

.g-card__title {
    color: var(--color-text-primary);
    font-weight: 400;
    font-size: 1.125rem;
    /* 18px */
    text-transform: lowercase;

}

.g-card-footer {
    position: absolute;
    bottom: -0.75rem;
    left: 50%;
    transform: translateX(-50%);
    width: fit-content;
    background-color: var(--color-bg-white);
    z-index: 2;
    display: flex;
    align-items: center;
    gap: 2rem;
    min-height: 1.5rem;
}

.g-card-footer__action-button {
    background-color: transparent;
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 !important;
}

.g-card-footer__flip-button {
    font-size: .875rem;
    font-weight: 400;
    line-height: 1.125rem;
    /* 18px */
    color: var(--color-text-primary);
    letter-spacing: .15rem;
    text-transform: uppercase;
    font-weight: 700;
    background-color: var(--color-bg-white);
}

.g-card.g-card--flipped .g-card-container {
    transform: rotateY(180deg);
}
</style>