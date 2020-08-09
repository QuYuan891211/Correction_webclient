<template>
  <div class="my-root">
    <header>
      <div class="main-title">
        <!-- header 加几个logo -->
        <div class="logo">
          <!-- <img src="static/nf-logo.png"> -->
          <img src="/images/logo/nmefclog.png" />
        </div>
        <ul>
          <li><router-link :to="{ path: '/' }">首页</router-link></li>
          <li>
            <router-link :to="{ path: '/content/Data' }"
              >数据订正</router-link
            >
          </li>
          <li>
            <router-link :to="{ path: '/content/Text' }">文本订正</router-link>
          </li>
          <li>
            <router-link :to="{ path: '/content/History' }"
              >历史记录</router-link
            >
          </li>
                    <li>
            <router-link :to="{ path: '/content/Product' }"
              >产品制作</router-link
            >
          </li>
        </ul>
      </div>
    </header>
    <div class="my-content">
      <div class="minor-title">
        <h1>预警报智能订正系统</h1>
        <!-- <button class="btn btn-default">Download Data</button> -->
        <button class="btn btn-default">
          <router-link :to="{ path: '/content/data' }"
            >登录</router-link
          >
        </button>
      </div>
    </div>
    <footer>
      <!-- 选择成员国的友情链接 -->
      <div class="db-select">
        <div class="dropdown">
          <button
            class="btn btn-default dropdown-toggle"
            type="button"
            id="db"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="true"
          >
            选择链接
            <span class="caret"></span>
          </button>
          <ul class="dropdown-menu">
          <li><a href="http://www.nmefc.cn/">国家海洋环境预报中心</a></li>
         <li><a href="http://www.oceanguide.org.cn/IndexHome">中国海洋预报网</a></li>
         <li><a href="http://neargoos.nmefc.cn:8080/exchange/index.jsp">neargoos中国实时数据库</a></li>
          </ul>
        </div>
      </div>

      <!-- 版权 -->
      <div class="copyright">
                          <div class="title">
         <h4> 版权所有 </h4>
        </div>
      <h4>
        Copyright ©2020 国家海洋环境预报中心 
      </h4>
      </div>
    </footer>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'home',
  components: {
    HelloWorld,
  },
};
</script>

<style scoped lang="less">
@import '../styles/base.less';
.my-root {
  background: url('/images/background/home_3.png');
  background-size: 100% auto;
  display: flex;
  min-height: 100vh;
  flex-direction: column;
  .my-content {
    display: flex;
    flex: 10;
    // transition: all 0.25s linear;
    // background: #7f9943;
    .minor-title {
      @minortitle();

      h1 {
        font-size: 5rem;
      }
      button {
        width: 200px;
        a {
          // text-decoration: none;
          // color: white;
          font-size: 2rem;
          @alinkdefault();
        }
      }
    }
  }
}

.my-content ::before h2,
.my-content ::before h4 {
  transform: translate3d(0, -30%, 0) translate3d(0, -30px, 0);
  transition: all 0.25s linear;
  background: linear-gradient(
    to bottom,
    rgba(72, 76, 97, 0) 0%,
    rgba(72, 76, 97, 0.8) 75%
  );
}

.my-content:hover {
  // opacity: 0.2;
  background-color: rgba(174, 174, 185, 0.15);
  transition: all 1s linear;
}

.my-content:hover h2,
.my-content:hover h4 {
  transform: translate3d(0, -30%, 0) translate3d(0, -30px, 0);
  transition: all 0.25s linear;
  color: #ffffff;
  // transform: translate3d(0, 0, 0);
}

.my-content h2,
.my-content h4 {
  transform: translate3d(0, 0, 0) translate3d(0, 0, 0);
  transition: all 0.25s linear;
  color: #ffffff;
  // transform: translate3d(0, 0, 0);
}

.my-content:hover .minor-title {
  background: rgba(27, 144, 125, 0.411);
  transition: all 0.25s linear;
  font-size: 8rem;
}
.my-content:hover .minor-title button {
  background: rgba(212, 132, 28, 0.815);
  transform: translate3d(0, 10%, 0) translate3d(0, 10px, 0);
  transition: all 0.25s linear;
}

// 沿主轴居中常量
@centered: {
  display: flex;
  justify-content: center;
};

// 含图片的背景
@basecontent: {
  //   height: 600px;
  //   display: flex;
};
// 所有的描述性文字的样式（主要是行高）
@desfont: {
  line-height: 1.8;
  font-family: Arial, Helvetica, sans-serif;
  text-shadow: 2px 2px 10px #000;
  // color: #e9d14c;
};

footer {
  height: 100px;
  /* flex: 1; */
  //   background: darkcyan;
}

@minortitle: {
  display: flex;
  color: whitesmoke;
  // flex: row;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  font-size: 20px;
  font-size: 600;
  @basecontent();
  h2 {
    @desfont();
  }
  h4 {
    width: 50%;
    @desfont();
  }
  button {
    // background: rgba(212, 132, 28, 0.815);
    background: rgba(27, 107, 199, 0.815);
    color: white;
    font-size: 20px;
    transform: translate3d(0, 0, 0) translate3d(0, 0, 0);
    transition: all 0.25s linear;
  }
};

// header的父类
@header: {
  // div底图
  // 上面url tab
  // 中间居中title
  // 下面一个button
  //   background: url("image/home_1_2.jpg");
  // height: 100px;
  display: flex;
  flex: 2;
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
    // background: rgba(0, 0, 0, 0.1);
    // background: red;
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
};
//footer的父类
@footer: {
  //   background: url("image/home_1_2.jpg");
  background: rgba(47, 221, 221, 0.329);
  display: flex;
  flex: 1;
  justify-content: space-around;
  align-items: center;
  .db-select {
    display: flex;
    flex: 1;
    @centered();
    .btn {
      border-radius: 8px;
      background-color: white;

      border: none;
      color: black;
      padding: 1px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 4px 2px;
      cursor: pointer;
    }
    .dropdown-menu a {
      color: black;
      font-size: 16px;
      text-decoration: none;
      display: block;
      width: 300px;
      text-align: center;
    }
    // 鼠标悬停更改下拉框颜色
    .dropdown-menu a:hover {
      background-color: rgba(215, 215, 215, 0.7);
    }
  }
  .members {
    display: flex;
    flex-direction: column;
    flex: 1;
    justify-content: center;
    h4 {
      color: white;
      @centered();
      @basefontshadow();
    }
    .members-img {
      @centered();
      img {
        margin-right: 0.25em;
      }
    }
  }
  .copyright {
    display: flex;
    flex-direction: column;
    flex: 1;
    justify-content: center;
    h4 {
      color: white;
      @basefontshadow();
    }
  }
};

// 定义的 白色的描述性字体
.deswhite() {
  color: aliceblue;
  font-size: 600;
}
header {
  // .header();
  // transform: translate3d(0, 0, 0) translate3d(0, 0, 0);
  // transition: all 0.25s linear;

  // animation-delay: 1s;
  @header();
}
header {
  .logo {
    animation: logoFadeIn 1.5s linear;
  }
  ul {
    animation: titleFadeIn 1.5s linear;
  }
}

@keyframes logoFadeIn {
  0% {
    opacity: 0;
  }
  25% {
    opacity: 0.25;
  }
  50% {
    opacity: 0.5;
  }
  75% {
    opacity: 0.75;
  }
}

@keyframes titleFadeIn {
  0% {
    opacity: 0;
    // transform: translate3d(0, -50%, 0);
    transform: translateY(-50%);
    // color: #ffffff;
  }
  25% {
    opacity: 0.25;
    // transform: translate3d(0, -37.5%, 0);
    transform: translateY(-37.5%);
    // color: #cabb36;
  }
  50% {
    opacity: 0.5;
    // transform: translate3d(0, -25%, 0);
    transform: translateY(-25%);
    // color: #3dac11;
  }
  75% {
    opacity: 0.75;
    // transform: translate3d(0, -12.5%, 0);
    transform: translateY(-12.5%);
    // color: #2c55da;
  }
}

header::before {
  transform: translate3d(0, -30%, 0) translate3d(0, -30px, 0);
  transition: all 0.25s linear;
}

footer {
  @footer();
  //   @footer();
}
</style>
