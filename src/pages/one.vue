<template>
  <div class="one">
    <!-- 列表 -->
    <div class="BossOne">
      <ul class="OneUl">
        <li
          v-for="(item,index) in list"
          :key="item.id"
          :class="{'active':iNow === index}"
          @click="btn(index,item._id)"
        >{{ item.title }}</li>
      </ul>
    </div>

    <!-- 升降序 -->
    <ul class="sheng">
      <li>综合</li>
      <li>销量</li>
      <li>新品</li>
      <li @click="JJ">价格</li>
      <li class="L">
        <p>∧</p>
        <p>∨</p>
      </li>
    </ul>

    <!-- 全部数据 -->
    <ul class="TwoUl">
      <li v-for="item in jiu" :key="item.id">
        <img :src="item.pic" alt />
        <p>{{ item.title }}</p>
        <div class="shang">
          <b>{{ item.price }}</b>
          <van-icon name="shopping-cart" is-link @click="showPopup" />
        </div>
      </li>
    </ul>

    <!-- 弹出层 -->
    <van-popup v-model="show" position="bottom" :style="{ height: '50%' }" closeable>
      <van-card
        num="2"
        price="600.00"
        desc="老中青 ...均可穿"
        title="精品男士睡衣"
        thumb="https://img.yzcdn.cn/vant/ipad.jpeg"
      >
        <template #tags>
          <van-tag plain type="danger">舒适</van-tag>
          <van-tag plain type="danger">活力四射</van-tag>
        </template>
        <template #footer>
          <van-button size="mini">推荐</van-button>
          <van-button size="mini">分享</van-button>
        </template>
      </van-card>
      <b class="b" @click="tan">确认</b>
    </van-popup>
  </div>
</template>
<script>
export default {
  data() {
    return {
      list: [],
      jiu: [],
      iNow: 0,
      sorts: true,
      show: false,
    };
  },
  async created() {
    let D = await this.axios.get("../../static/json/fenlei.json");
    this.list = D.data.result;
  },
  methods: {
    tan() {
      this.$toast.success("购买成功");
      this.show = false;
    },
    btn(index, id) {
      this.iNow = index;
      this.axios.get("../../static/json/list.json").then((msg) => {
        let A = msg.data.result.filter((item) => {
          return item.cid == id;
        });
        this.jiu = A;
      });
    },
    JJ() {
      if (this.sorts == true) {
        this.sorts = false;
        this.jiu.sort((A, B) => {
          return A.price - B.price;
        });
      } else {
        this.sorts = true;
        this.jiu.sort((A, B) => {
          return B.price - A.price;
        });
      }
    },
    showPopup() {
      this.show = true;
    },
  },
  mounted() {
    this.axios.get("../../static/json/list.json").then((msg) => {
      this.jiu = msg.data.result;
      console.log(this.jiu);
    });
  },
};
</script>
<style lang="less">
.one {
  .b {
    width: 4rem;
    height: 0.5rem;
    display: block;
    text-align: center;
    line-height: 0.5rem;
    background-color: sienna;
    color: #fff;
    position: fixed;
    bottom: 0;
  }
  .active {
    color: #000 !important;
    border-bottom: 0.02rem solid #000;
    line-height: 0.2rem;
    height: 0.2rem;
  }
  .sheng {
    width: 4rem;
    display: flex;
    align-items: center;
    line-height: 0.4rem;
    position: relative;

    li {
      margin-left: 0.2rem;
      color: #ccc;
    }
    .L {
      position: absolute;
      padding: 0;
      left: 1.7rem;
      top: -0.06rem;
      p {
        margin: 0;
        height: 0.1rem;
      }
    }
  }
  .BossOne {
    width: 4rem;
    overflow: auto;
    .OneUl {
      width: 7rem;
      height: 0.4rem;
      overflow: auto;
      display: flex;
      align-items: center;
      justify-content: space-around;
      li {
        color: cornflowerblue;
      }
    }
  }
  .TwoUl {
    background-color: #f5f5f5;
    width: 4rem;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    padding-bottom: 0.2rem;
    li {
      width: 1.8rem;
      height: 2rem;
      overflow: hidden;
      background-color: #fff;
      margin-top: 0.1rem;
      border-radius: 0.06rem;
      img {
        width: 1.2rem;
        margin: 0 auto;
        height: 1.2rem;
        margin-top: 0.1rem;
        border-radius: 0.06rem;
      }
      p {
        text-align: center;
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
        color: rgb(99, 89, 89);
        line-height: 0.3rem;
        margin: 0 auto;
        width: 1.2rem;
      }
      .shang {
        width: 1.2rem;
        margin: 0 auto;
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 0.3rem;
        b {
          color: crimson;
        }
        .van-icon-shopping-cart::before {
          content: "\F0BC";
          color: rgb(228, 226, 226);
        }
      }
    }
  }
}
</style>