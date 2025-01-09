<script setup lang="ts">
defineProps({
  showHeader: {
    type: Boolean,
    default: true,
  },
});
</script>

<template>
  <header class="header" v-if="showHeader">
    <div class="header__assistant">
      <div class="header__assistant-avatar">
        <img inert src="/assets/public/images/avatar.webp" width="60" height="60" alt="Assistant" />
      </div>
      <div class="header__assistant-text">
        <h2 class="header__assistant-name">Sara K.</h2>
        <div inert class="header__assistant-status">
          <mark></mark>
          <span>online</span>
        </div>
      </div>
    </div>
    <span class="header__logo hidden-tablet">Offer name</span>
  </header>
</template>
