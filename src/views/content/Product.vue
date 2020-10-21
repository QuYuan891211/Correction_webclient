<template>
  <div class="center">
    <div class="center-header">
      <div class="title">数据订正</div>
      <br />
    </div>
    <div class="center-center">
      <ul>
        <li>
          <el-select
            v-model="product_selected"
            value-key="id"
            placeholder="选择产品生成任务"
            @change="checkProduct()"
          >
            <el-option
              v-for="item in product_list"
              :key="item.id"
              :label="item.proName"
              :value="item"
            ></el-option>
          </el-select>

          <el-button
            class="buttonType2"
            type="success"
            plain
            @click="doProduct"
            :disabled="makeDisable"
          >生成产品</el-button>
          <el-button
            class="buttonType2"
            type="success"
            plain
            @click="downloadFile"
            :disabled="downloadDisable"
          >
            <a download="filename"></a>下载预览
          </el-button>
        </li>
        <li>
          生成状态：
          <span>{{produce_success}}</span>
        </li>
        <li>
          最后生成时间：
          <span>{{produce_time}}</span>
        </li>
        <li>
          生成者：
          <span>{{producer}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
//必须
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
//
import axios from 'axios';
import JSZip from 'jszip';
import FileSaver from 'file-saver';
import moment from 'moment';
import countTo from 'vue-count-to';
//
const host = 'http://localhost:8082';
//必要
@Component({
  //
})
export default class DataView extends Vue {
  product_list: any = []; //产品选择的列表
  product_selected: any = null; //产品选择结果
  produce_time: any = null; //产品最后生成时间
  producer: any = null; //产品生成者
  geneRecord: any = null; //产品生成相关信息
  produce_success: any = null; //产品生成结果
  downloadDisable: any = true; //下载键禁用开关
  makeDisable: any = true; //下载键禁用开关
  // lifecycle hook
  mounted() {
    this.loadProduct();

    //this.mission_list=getAllSource();

    //alert(this.mission_list.code);
  }
  loadProduct() {
    let url = `${host}/Product/proInfoList`;

    axios
      .get(url, {
        params: this.product_list,
      })
      .then((res) => {
        //alert(res.data.code);
        if (res.data.code == 'OK') {
          this.product_list = res.data.result;
        } else {
          alert(res.data.msg);
        }
      })
      .catch((err) => {
        alert('err');
      });
  }
  checkProduct() {
    let url = `${host}/Record/getRecord`;
    let date = new Date();
    let par = { proId: this.product_selected.id, date: date.toISOString() };
    //alert(this.product_selected);
    axios
      .post(url, par)
      .then((res) => {
        //alert(res.data.code);
        if (res.data.code == 'OK') {
          this.geneRecord = res.data.result[0];
          if (this.geneRecord != null) {
            if (this.geneRecord.isok == 1) {
              this.produce_success = '成功';
              this.downloadDisable = false;
            } else {
              this.produce_success = '未成功';
              this.downloadDisable = true;
            }
            this.producer = this.geneRecord.forecaster;
            this.produce_time = this.geneRecord.geneTime;
          } else {
            this.producer = null;
            this.produce_time = null;
            this.produce_success = '未生成';
            this.downloadDisable = true;
          }
          this.makeDisable=false;
        } else {
          alert(res.data.msg);
          this.makeDisable=true;
        }
      })
      .catch((err) => {
        this.makeDisable=true;
        alert('err');
      });
  }

  doProduct() {
    let url = `${host}/Maker/fileProcess`;
    axios
      .post(url, this.product_selected)
      .then((res) => {
        //alert(res.data.code);
        if (res.data.code == 'OK') {
          this.geneRecord = res.data.result;
          if (this.geneRecord != null) {
            if (this.geneRecord.isok == 1) {
              this.produce_success = '成功';
              this.downloadDisable = false;
            } else {
              this.produce_success = '未成功';
              this.downloadDisable = true;
            }
            this.producer = this.geneRecord.forecaster;
            this.produce_time = this.geneRecord.geneTime;
          } else {
            this.producer = null;
            this.produce_time = null;
            this.produce_success = '未生成';
            this.downloadDisable = true;
          }
        } else {
          alert(res.data.msg);
        }
      })
      .catch((err) => {
        alert('err');
      });
  }

  downloadFile() {
    let url = `${host}/Maker/download`;
    axios
      .post(url, this.geneRecord, {
        responseType: 'blob',
      })
      .then((res) => {
        let blob = res.data;
        var fileName = res.headers;
        fileName = fileName['content-disposition'];
        fileName = fileName.split(';')[1].split('filename=')[1];
        fileName = decodeURI(fileName);

        let reader = new FileReader();
        reader.readAsDataURL(blob);
        reader.onload = (e) => {
          let a = document.createElement('a');
          a.download = fileName; //'test.txt';//fileName;
          a.href = e.target.result;
          document.body.appendChild(a);
          a.click();
          document.body.removeChild(a);
        };
      });
    // var xhr = new XMLHttpRequest();
    // xhr.open('POST', url, true); // 也可以使用POST方式，根据接口
    // xhr.responseType = 'blob'; // 返回类型blob
    // // 定义请求完成的处理函数，请求前也可以增加加载框/禁用下载按钮逻辑
    // xhr.onload = function () {
    //   // 请求完成
    //   if (this.status === 200) {
    //     // 返回200
    //     var blob = this.response;
    //     var reader = new FileReader();
    //     reader.readAsDataURL(blob); // 转换为base64，可以直接放入a表情href
    //     reader.onload = function (e) {
    //       // 转换完成，创建一个a标签用于下载
    //       var a = document.createElement('a');
    //       a.download = 'data.xlsx';
    //       a.href = e.target.result;
    //       $('body').append(a); // 修复firefox中无法触发click
    //       a.click();
    //       $(a).remove();
    //     };
    //   }
    // };
    // // 发送ajax请求
    // xhr.send();

    // let url = `${host}/Maker/download`;
    // axios
    //   .post(url, this.geneRecord)
    //   .then((res) => {
    //     let data=res.data;
    //     const blob=data;
    //     var filename='test';
    //     const elink=document.createElement('a');
    //     elink.download=filename;
    //     elink.href=URL.createObjectURL(blob);
    //     document.body.appendChild(elink);
    //     elink.click();
    //     URL.revokeObjectURL(elink.href);
    //     document.body.removeChild(elink);

    //   })
    //   .catch((err) => {
    //     alert('err');
    //   });
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
        //text-shadow: 2px 2px 10px #000;
        // .el-input{
        //     background: #ffffff; //红色
        //     color: rgb(231, 17, 17);

        // }
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
        .result_table {
          height: 500px;
          overflow: scroll;
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
      .card-form {
        display: flex;
        flex: 1;
        flex-direction: column;
        // 注意此处为副轴方
        font-family: Arial, Helvetica, sans-serif;
        text-shadow: 2px 2px 10px #000;
        .title {
          font-size: 2rem;
          color: white;
        }
        .num-box {
          column-count: 2;
          color: white;
          list-style: none;
          .num {
            font-size: 3rem;
          }
          .text {
            font-size: 1.5rem;
          }
        }
        .body {
          ul {
            li {
              line-height: 3em;
              color: white;
              font-size: 1rem;
              list-style: none;
              .buttonType1 {
                background-color: rgb(216, 191, 50);
                color: black;
              }
              .buttonType2 {
                background-color: rgb(230, 145, 34);
                color: black;
              }
            }
          }
        }
      }
    }
  }
}
</style>