<template>
  <div id="header">
    <div class="main-title">
      <!-- header 加几个logo -->
      <div class="logo">
        <!-- <img src="static/nf-logo.png"> -->
        <img src="/images/logo/nmefclog.png" />
      </div>
      <!-- <ul>
        <li>Home</li>
        <li>OverView</li>
        <li>Data</li>
        <li>Product</li>
      </ul>-->
      <ul>
        <li>
          <!-- <router-link :to="{ name: 'map', params: { kind: 'history' }}">船舶历史轨迹</router-link> -->
          <router-link :to="{ path: '/' }">首页</router-link>
        </li>
        <li>
          <router-link :to="{ path: '/content/Data' }"
            >数据订正</router-link
          >
        </li>
        <li>
          <router-link :to="{ path: '/content/Text' }">文本订正</router-link>
        </li>
        <li>
          <router-link :to="{ path: '/content/History' }">历史记录</router-link>
        </li>
          <li>
          <router-link :to="{ path: '/content/Product' }">产品制作</router-link>
        </li>
      </ul>
    </div>
    <div class="minor-title">
      <h1>预警报智能订正系统</h1>
      <!-- <button class="btn btn-default">Download Data</button> -->
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
@Component({})
export default class Header extends Vue {}
</script>
<style scoped lang="less">
// @import '../styles/base.less';
@import '../../styles/base.less';
.my-root {
  background: rgb(138, 128, 114);
}

// header的父类
.header() {
  // div底图
  // 上面url tab
  // 中间居中title
  // 下面一个button
  background: url('/images/background/header.png');
  background-size: 100% 100%;
  // height: 100px;
  display: flex;
  flex-direction: column;
  //底部增加一个内padding
  padding-bottom: 50px;
  .logo {
    img {
      height: 100px;
    }
  }
  .main-title {
    display: flex;
    flex-direction: row;
    justify-content: center;
    color: wheat;
    font-weight: 700;
    font-size: 30px;
    align-items: center;
    @baseheader();
    ul {
      display: flex;
      flex-direction: row;

      li {
        padding-right: 1em;
        // 去掉li的圆点
        list-style: none;
        a {
          color: white;
        }
      }
    }
  }
  .minor-title {
    display: flex;
    color: whitesmoke;
    // flex: row;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    font-size: 1000;
    margin: 0.5rem;
    h2 {
      @basefontshadow();
      @maintitlefont();
      // @maintitlefont();
    }
    h4 {
      width: 50%;
    }
    // button {
    //   background: rgba(212, 132, 28, 0.815);
    //   color: white;
    //   font-size: 20px;
    //   a {
    //     // text-decoration: none;
    //     // color: white;
    //     @alinkdefault();
    //   }
    // }
  }
}

// 含图片的背景
.basecontent() {
  height: 600px;
  display: flex;
  font-family: Arial, Helvetica, sans-serif;
}

// 所有的描述性文字的样式（主要是行高）
.desfont() {
  line-height: 1.8;
}
// 定义的 白色的描述性字体
.deswhite() {
  color: aliceblue;
  font-size: 600;
}
#header {
  .header();
}

.content-1-title span {
  display: flex;
  justify-content: center;
  flex: 3;
  color: rgba(199, 190, 159, 0.801);
  font-size: 600;
}
</style>
