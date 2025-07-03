<template>
    <header class="g-header">
        <section class="g-header-left">
            <h1 class="g-header__title">
                {{ title }}
            </h1>
        </section>

        <section class="g-header-right">
            <slot name="right"></slot>
        </section>
    </header>
</template>

<script setup>
defineProps({
    title: {
        type: String,
        required: true,
    }
})
</script>

<style scoped>
.g-header {
    width: 100%;
    height: 120px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 3rem 4.125rem;
    box-sizing: border-box;
}

.g-header__title {
    text-transform: uppercase;
    font-weight: 700;
    font-size: 16px;
    line-height: 24px;
    letter-spacing: 12%;
}
</style>
