<template>
  <button class="g-button">
    <slot />
  </button>
</template>

<script setup></script>

<style scoped>
.g-button {
  min-width: 23.26vw;
  font-size: 1.5rem; /* 24px */
  font-weight: 400;
  line-height: 150%; /* 36px */
  padding: .625rem 1rem; /* 10px 16px */
  background-color: var(--color-primary);
  color: var(--color-text-secondary) !important;
  border-radius: 100px;
}

.g-button:hover {
  background-color: #006FCB;
}
</style>