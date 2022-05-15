<template>
  <div class="app">
    <Header />
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";

export default {
  components: {
    Header,
  },
};
</script>

<style scoped>
.app {
  background: #f3f3f3;
  width: 100vw;
  min-height: 100vh;
}
</style>