<script setup lang="ts">
import AppHeader from './components/AppHeader.vue';
import CalendarContainer from './components/CalendarContainer.vue';

</script>

<template>
  <div class="wrapper">
    <AppHeader />
    <main class="main-content">
      <CalendarContainer />
    </main>
  </div>
</template>

<style lang="scss">
@use "./styles/partials/variables" as *;
@use "./styles/general.scss";

.wrapper {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;

  .main-content {
    padding-top: 1.5rem;
    flex-grow: 1;
    background-color: $bg-color;
  }
}
</style>
