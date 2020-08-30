<template>
  <div class="center">
    <div class="center-header">
      <!-- 统计的title -->
      <div class="title">文本订正</div>
      <br />
    </div>
    <div class="center-center">
      <div class="center-center-left">
        <div class="card-form">
          <div class="sub-title">文本信息：</div>
          <div class="body">
            <ul>
              <li>
                创建日期:
                <!-- <span>{{text_result.beginTime}}</span> -->
              </li>
              <li>
                更新日期:
                <!-- <span>{{text_result.endTime}}</span> -->
              </li>
              <li>
                是否已审核:
                <!-- <span>{{text_result.isCheck}}</span> -->
              </li>
              <li>
                预报员:
                <!-- <span>{{text_result.forcaster}}</span> -->
              </li>
              <li>
                审核人:
                <!-- <span>{{text_result.checker}}</span> -->
              </li>
              <li>
                <el-button
                type="info"
                class="btn_loadText"
                @click="loadText"
              >加载前一日文本内容</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_confirm_forcaster"
                @click="loadText"
              >预报员确认完成</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_confirm_checker"
                @click="loadText"
              >审核员确认完成</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_cancel_checker"
                @click="loadText"
              >审核员取消确认</el-button>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- 右侧内容 -->
      <div class="center-center-right">
        <div class="content-form">
                              <!-- 搜索条件form -->
        <el-form>
          <ul>
            <li>
          <div class="title-select-mission">选择模板：</div>
          </li>
          <li>
          <div class="form-select-mission">

          <el-form-item>
            <el-select v-model="mission_selected" value-key="id" placeholder="请选择文本模板">
              <el-option
                v-for="item in mission_list"
                :key="item.id"
                :label="item.name"
                :value="item.id"
              ></el-option>
            </el-select>
          </el-form-item>

          </div>
          </li>
          <li>
          <div class="title-text">第一天预报文本：</div>
          </li>
          <li>

              <el-form-item>
    <el-input type="textarea" :rows="2" v-model="data1"></el-input>
</el-form-item>

          </li>
                    <li>
          <div class="title-text">第二天预报文本：</div>
          </li>
          <li>

              <el-form-item>
    <el-input type="textarea" :rows="2" v-model="data1"></el-input>
</el-form-item>

          </li>
                    <li>
          <div class="title-text">第三天预报文本：</div>
          </li>
          <li>

              <el-form-item>
    <el-input type="textarea" :rows="2" v-model="data1"></el-input>
</el-form-item>

          </li>
          </ul>
        </el-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import axios from 'axios';
import JSZip from 'jszip';
import FileSaver from 'file-saver';
import moment from 'moment';
import countTo from 'vue-count-to';
const host = 'http://localhost:8083';
@Component({
  components: { countTo },
})
export default class TextView extends Vue {
  mission_selected: any = null;
  mission_list: any = [];
  text_result: any = null;
  url: any = null;
  tempdata: any = null;
  pictLoading = false;
  loadingBtn = false;
  data1 : any = null;
  // lifecycle hook
  mounted() {}

  //computed
  get computedTest() {
    return null;
  }

  //methods
  loadText(){

  }
}
</script>
<style scoped lang="less">
@import '../styles/base.less';
@margindefault: {
  margin: 1em;
};
@thumbnail: {
};
@borderradius: {
  border-radius: 1em;
};
//蓝色背景
@bluebackground: {
  background: rgba(51, 204, 204, 1);
};
//灰色背景
@graybackground: {
  background: rgba(215, 215, 215, 0.7);
};
// 统一白色form中的font样式
@whitefont: {
  font-family: Arial, Helvetica, sans-serif;
  text-shadow: 2px 2px 10px #000;
  color: white;
};
// 为二级标题加了一个左+上的间距
@minortitle: {
  // 统计title加一个左+上的间距
  margin-left: 1.5em;
  margin-top: 1em;
  font-size: 200%;
  color: white;
  font-family: Arial, Helvetica, sans-serif;
  text-shadow: 2px 2px 10px #000;
};
// 中间的区域
.center {
  flex: 1;
  display: flex;
  flex-direction: column;
  //蓝色背景
  @bluebackground();
  @margindefault();
  @borderradius();
  // 中间部分
  // 横向排列
  .center-header {
    display: flex;
    flex: 3;
    // background: rgba(39, 216, 216, 0.897);

    flex-direction: column;

    .title {
      display: flex;
      justify-content: flex-start;
      font-size: 6rem;
      @minortitle();
    }
  }

  .center-center {
    display: flex;
    flex-direction: row;
    justify-content: center;
    .center-center-left {
      display: flex;
      flex-direction: column;
      .card-form {
        display: flex;
        flex: 1;
        flex-direction: column;
        // 注意此处为副轴方
        font-family: Arial, Helvetica, sans-serif;
        text-shadow: 2px 2px 10px #000;
        .sub-title {
          font-size: 1.5rem;
          color: white;
        }
        .body {
          ul {
            li {
              line-height: 3em;
              color: white;
              font-size: 1rem;
              list-style: none;
            }
          }
        }
      }
      // 非最后一个都加入右侧的边框（白色）
      .card-form:not(:last-of-type) {
        // 加入右侧的边框
        border-right: 1px solid white;
      }
    }
    .center-center-right {
      display: flex;
      flex-direction: column;
      .content-form {
        display: flex;
        flex: 1;
        flex-direction: column;
        // 注意此处为副轴方
        font-family: Arial, Helvetica, sans-serif;
        text-shadow: 2px 2px 10px #000;
          ul {
            li {
              line-height: 3em;
              color: white;
              font-size: 1rem;
              list-style: none;
            }
          }
        
      }
    }
  }
}
</style>
