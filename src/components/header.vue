<template>
  <div class="header">
    <img src="../assets/素材_01.jpg" alt />
    <van-search placeholder="请输入搜索关键词" @click="S" />
  </div>
</template>
<script>
export default {
  methods: {
    S() {
      this.$router.push({
        path: "/SS",
      });
    },
  },
};
</script>
<style lang="less">
.header {
  img {
    width: 4rem;
  }
}
</style>