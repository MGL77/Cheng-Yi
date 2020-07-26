<template>
  <div>
    <Header />
    <router-view />
    <van-tabbar v-model="active" active-color="#07c160" inactive-color="#000">
      <van-tabbar-item replace to="one" icon="diamond">首页</van-tabbar-item>
      <van-tabbar-item replace to="two" icon="wap-nav">分类</van-tabbar-item>
      <van-tabbar-item replace to="three" icon="shopping-cart">购物车</van-tabbar-item>
      <van-tabbar-item replace to="four" icon="map-marked">个人</van-tabbar-item>
    </van-tabbar>
  </div>
</template>
<script>
import Header from "@/components/header.vue";
export default {
  data() {
    return {
      active: 0,
    };
  },
  components: {
    Header,
  },
};
</script>
<style lang="less">
.van-tabbar--fixed {
  position: fixed;
  bottom: 0;
  left: 0;
  margin-top: 2rem;
}
</style>