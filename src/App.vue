<template>
  <div id="app">
    <router-view />
    <top />
  </div>
</template>

<script>
import top from "@/components/TopBottom.vue";
export default {
  components: {
    top,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  font-size: 0.12rem;
}
img {
  display: block;
}
</style>
