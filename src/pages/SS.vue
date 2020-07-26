<template>
  <div class="search">
    <img src="../assets/素材_01.jpg" alt />
    <b @click="fan" class="fan">
      <van-icon name="arrow-left" />&nbsp;搜索
    </b>
    <!-- 顶部搜索栏 -->
    <div class="header">
      <!-- <div class="head-title title-style">输入关键字</div> -->
      <div class="head-input">
        <input
          type="text"
          ref="inputchange"
          v-model="valuetext"
          @keyup.enter="onSearch(true)"
          @keyup.tab="onSearch(true)"
          @focus="initPage"
          placeholder="请输入关键字进行搜索"
        />
        <div type="text" @click="clear" class="input-btn title-style">清除</div>
      </div>

      <div class="history-panel" v-if="!isFocus">
        <div v-if="historyxs" style="color:#ccc">搜索历史</div>
        <div v-if="searches_list.length>0">
          <ul class="his_ulcon" v-if="historyxs">
            <li
              v-for="(item,index) in searches_list"
              :key="index"
              @click="historysearch(item)"
            >{{item}}</li>
          </ul>
        </div>
        <div class="history-tips" v-else>暂无搜索记录！</div>
        <p v-if="historyxs" @click="clearhis">清空历史记录</p>
      </div>
    </div>

    <b class="b">热门搜索</b>
    <ul class="ul">
      <li>夹克</li>
      <li>牛仔帽</li>
      <li>沙地摩托</li>
      <li>连衣裙</li>
      <li>精品石材</li>
      <li>大衣</li>
      <li>外套</li>
      <li>夹克</li>
      <li>牛仔帽</li>
      <li>沙地摩托</li>
      <li>连衣裙</li>
      <li>精品石材</li>
      <li>大衣</li>
      <li>外套</li>
    </ul>
    <!-- ... 此处省略无关代码 -->

    <!-- 底部按钮 -->
    <!-- <div class="footer title-style">
      <van-row>
        <van-col span="12">
          <div class="left" @click="resetData">重置所选条件</div>
        </van-col>
        <van-col span="12">
          <div class="right" @click="onSearch(true)">立即搜索</div>
        </van-col>
      </van-row>
    </div>-->
  </div>
</template>
 
<script type="text/Babel">
import { saveSearch } from "../../static/json/cache.js"; //引用本地存储js
import storage from "good-storage"; //引入good-storage包
export default {
  data() {
    return {
      isFocus: true,
      searches_list: [], //历史搜索记录列表
      historyxs: false,
      valuetext: "",
    };
  },
  mounted() {},
  methods: {
    fan() {
      this.$router.push({
        path: "/boss/one",
      });
    },
    //输入框获取焦点
    initPage() {
      this.isFocus = false;
      this.$emit("initSearchPage");
      //为避免重复先清空再添加
      this.searches_list = [];
      let searches = storage.get("_search_");
      this.searches_list = searches ? searches : [];
      if (this.searches_list.length > 0) {
        this.historyxs = true;
      } else {
        this.historyxs = false;
      }
    },
    //清空历史记录
    clearhis() {
      storage.remove("_search_");
      this.searches_list = [];
      this.historyxs = false;
    },
    //点击历史搜索把搜索的记录添加到good-storage中
    historysearch(item) {
      saveSearch(item);
      this.valuetext = item;
      this.historyxs = false;
    },
    resetData() {
      // ...
      //  此次省略重置数据的逻辑代码
    },
    onSearch(isFirst) {
      this.isFocus = true;
      if (this.valuetext != "") {
        //搜索框不为空
        saveSearch(this.valuetext); // 本地存储搜索的内容
        let params = {
          majorInfo: this.valuetext, //流程类型或者流程名称
        };
        this.$emit("handleSearch", params);
        this.isFocus = true;
      }
      // ...
      // 此次省略调用搜索接口的代码
    },
    clear() {
      this.valuetext = "";
    },
    // ... 无关代码已省略
  },
};
</script>
 
<style lang="less" rel="stylesheet/less" type="text/less" scoped>
img {
  width: 4rem;
}
.b {
  line-height: 0.4rem;
  color: #ccc;
}
.fan {
  display: flex;
  align-items: center;
  line-height: 0.4rem;
  color: #000;
}
.ul {
  display: flex;
  flex-wrap: wrap;
  li {
    padding: 0.06rem 0.06rem;
    border-radius: 0.02rem;
    text-align: center;
    background-color: #f5f5f5;
    margin-right: 0.2rem;
    margin-bottom: 0.1rem;
    color: #8d8888;
  }
}
.search {
  overflow-y: scroll;
  overflow-x: hidden;
  padding: 0.14rem 0.12rem 0.53rem;
  .header {
    border-bottom: 0.01rem solid #f2f2f2;
    .head-title {
      padding-bottom: 0.05rem;
      color: #666666;
    }
    .head-input {
      width: 100%;
      padding-bottom: 0.1rem;
      display: flex;
      flex-direction: row;
      justify-content: space-between;

      > input {
        height: 0.36rem;
        width: 3rem;
        border-radius: 0.03rem;
        font-family: PingFang-SC-Regular;
        font-weight: Regular;
        border: 0.01rem solid #f5f5f5;
        color: #999999;
        font-size: 0.12rem;
        padding-left: 0.12rem;
      }
      .input-btn {
        color: #ccc;
        width: 0.5rem;
        height: 0.36rem;
        line-height: 0.36rem;
        text-align: center;
      }
      .input-btn:hover {
        color: steelblue;
      }
    }
    .history-panel {
      width: 100%;
      overflow: hidden;
      padding: 0.1rem 0;
      border-top: 1px solid #f2f2f2;
      .his_ulcon {
        margin-top: 0.1rem;
        box-sizing: border-box;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        flex-wrap: wrap;
        > li {
          border: 1px solid #f2f2f2;
          border-radius: 0.03rem;
          display: inline-block;
          font-size: 0.12rem;
          padding: 0 0.15rem;
          width: fit-content; //div宽度自适应文字内容
          height: 0.29rem;
          line-height: 0.29rem;
          text-align: center;
          margin-right: 0.1rem;
          background-color: #f5f5f5;
          margin-bottom: 0.1rem;
        }
      }
      div {
        box-sizing: border-box;
        font-size: 0.13rem;
        color: #666666;
        font-weight: Medium;
        font-family: PingFang-SC-Medium;
      }
      > p {
        text-align: center;
        margin-top: 0.1rem;
        font-size: 0.13rem;
      }
    }
    .history-tips {
      text-align: center;
    }
  }
  .title-style {
    font-size: 0.13rem;
    font-weight: Medium;
    font-family: PingFang-SC-Medium;
  }
}
</style>