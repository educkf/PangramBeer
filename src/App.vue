<template>
  <div id="app">
    <AppHeader />
    <AppContent />
    <AppFooter />
  </div>
</template>

<script>
import AppHeader from '@/components/AppHeader.vue';
import AppContent from '@/components/AppContent.vue';
import AppFooter from '@/components/AppFooter.vue';

export default {
  name: "app",
  components: {
    AppHeader,
    AppContent,
    AppFooter
  }
};
</script>

<style lang="scss">
@import 'src/assets/_base.scss';
@import 'src/assets/_variables.scss';



</style>
