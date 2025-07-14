<template>
    <div class="g-card" :class="{ 'g-card--flipped': isFlipped }">
        <div class="g-card-container">
            <!-- Лицевая сторона -->
            <div class="g-card-frontface">
                <div class="g-card-wrapper">
                    <div class="g-card__order">{{ cardNumber }}</div>
                    <div class="g-card__title">{{ data.word }}</div>

                    <div class="g-card-footer">
                        <button type="button" class="g-card-footer__flip-button"
                            @click="onClickFlip">Перевернуть</button>
                    </div>
                </div>
            </div>

            <!-- Обратная сторона -->
            <div class="g-card-backface">
                <div class="g-card-wrapper">
                    <div class="g-card__order">{{ cardNumber }}</div>
                    <template v-if="data.status !== CARD_STATUS_PENDING_VALUE">
                        <div class="g-card__status">
                            <GIconAcceptBigger v-if="data.status === CARD_STATUS_SUCCESS_VALUE" />
                            <GIconRejectBigger v-else />
                        </div>
                    </template>
                    <div class="g-card__title">{{ data.translation }}</div>

                    <div class="g-card-footer">
                        <template v-if="data.status === CARD_STATUS_PENDING_VALUE">
                            <button type="button" class="g-card-footer__action-button" @click="onCardAccept">
                                <GIconAccept />
                            </button>
                            <button type="button" class="g-card-footer__action-button" @click="onCardReject">
                                <GIconReject />
                            </button>
                        </template>

                        <p class="g-card-footer__complete-label" v-else>Завершено</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed } from 'vue';
import GIconReject from './icons/GIconReject.vue';
import GIconAccept from './icons/GIconAccept.vue';
import GIconAcceptBigger from './icons/GIconAcceptBigger.vue';
import GIconRejectBigger from './icons/GIconRejectBigger.vue';
import { CARD_STATE_OPENED_VALUE, CARD_STATUS_SUCCESS_VALUE, CARD_STATUS_PENDING_VALUE } from '../constants';

const emit = defineEmits(['flip', 'accept', 'reject']);

const props = defineProps({
    data: {
        type: Object,
        default: null
    }
})

const cardNumber = computed(() => {
    return props.data.id < 10 ? `0${props.data.id}` : props.data.id;
})
const isFlipped = computed(() => {
    return props.data.state === CARD_STATE_OPENED_VALUE
})

function onClickFlip() {
    emit('flip', props.data.id)
}

function onCardAccept() {
    emit('accept', props.data.id)    
}

function onCardReject() {
    emit('reject', props.data.id)
}

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

.g-card-frontface {
    backface-visibility: hidden;
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

.g-card__status {
    position: absolute;
    top: -1rem;
    left: 50%;
    transform: translateX(-50%);
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
    cursor: pointer;
}

.g-card-footer__flip-button {
    font-size: .875rem;
    line-height: 1.125rem;
    color: var(--color-text-primary);
    letter-spacing: .15rem;
    text-transform: uppercase;
    font-weight: 700;
    background-color: var(--color-bg-white);
    cursor: pointer;
}

.g-card-footer__complete-label {
    font-size: .875rem;
    font-weight: 400;
    line-height: 1.125rem;
    color: var(--color-text-primary);
    letter-spacing: .15rem;
    text-transform: uppercase;
    font-weight: 700;
    margin: 0 !important;
}

.g-card.g-card--flipped .g-card-container {
    transform: rotateY(180deg);
}

/* Управление pointer-events для обеспечения кликабельности */
.g-card-frontface {
    pointer-events: auto;
}

.g-card-backface {
    pointer-events: none;
}

.g-card.g-card--flipped .g-card-frontface {
    pointer-events: none;
}

.g-card.g-card--flipped .g-card-backface {
    pointer-events: auto;
}
</style>