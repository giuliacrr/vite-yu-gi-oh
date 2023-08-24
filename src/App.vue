<script>
import TheHeader from "./components/Header.vue";
import TheFooter from "./components/Footer.vue";
import TheMain from "./components/Main.vue";

export default {
  components: {
    TheHeader,
    TheFooter,
    TheMain,
  },
};
</script>

<template>
  <TheHeader></TheHeader>
  <TheMain></TheMain>
  <TheFooter></TheFooter>
</template>

<style lang="scss"></style>
