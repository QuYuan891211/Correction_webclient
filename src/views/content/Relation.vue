<template>
  <div class="center">
    <div class="center-header">
      <div class="title">关系管理</div>
      <br />
    </div>
    <div class="center-center">
      <div class="center-center-left">
        <div class="card-form">
          <div class="sub-title">
            <!-- 表头内容 -->
            <!-- <div class="table table-striped table-bordered"> -->
            <el-table
              class="result_table"
              :data="dataList"
              ref="multipleTable"
              v-loading="this.pictLoading"
              element-loading-background="rgba(0, 0, 0, 0.5)"
              element-loading-text="Loading..........."
              element-loading-spinner="el-icon-loading"
              
            ><!-- @cell-click="cellClick" -->
              <el-table-column prop="mName" label="任务名称"></el-table-column>
              <el-table-column
                v-for="(item, index) in table_labels"
                :key="index"
                :prop="item.prop"
                :width="item.width"
                :label="item.label"
              >
                <template scope="scope">
                  <el-input
                    v-model="scope.row[scope.column.property]"
                    
                    @blur="loseFcous(scope.$index, scope.row,scope.column)"
                  ></el-input><!-- v-if="scope.row.seen===scope.column.property"-->
                  <!--<span style="margin-left: 10px" v-else>{{getTableText(scope.row,scope.column)}}</span> -->
                </template>
              </el-table-column>
            </el-table>
          </div>
        </div>
      </div>
      <div class="center-center-right">
        <div class="card-form">
          <div class="body">
            <ul>
              <li>
                <el-select
                  v-model="mission_selected"
                  value-key="mid"
                  placeholder="选择订正任务"
                  @change="loadTable()"
                >
                  <el-option
                    v-for="item in mission_list"
                    :key="item.mid"
                    :label="item.mName"
                    :value="item"
                  ></el-option>
                </el-select>
              </li>
              <li>
                任务日期:
                <span>{{creatTime}}</span>
              </li>
              <li>
                是否已审核:
                <span>{{checked}}</span>
              </li>
              <li>
                预报员:
                <span></span>
              </li>
              <li>
                审核人:
                <span>测试审核员</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
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
const host = 'http://localhost:8081';
//必要
@Component({
  //
})
export default class DataView extends Vue {
  mission_list: any = []; //任务选择的任务列表
  mission_selected: any = null; //任务选择结果
  //table_data: any = []; //表格数据内容
  daily_data: any = []; //每日数据（从后台请求的数据）
  table_labels: any = [
    { prop: 'sName', label: '请选择订正任务', width: '200', other: '-1' },
  ]; //表格标题
  creatTime: any = null; //任务日期
  dataList: any =[]; //每日数据 daily_data中的数据列表（之后升级用作表格内容）
  elementList: any = []; //要素信息列表
  //eLableList: String[] = []; //要素标题列表
  checked: any = null; //审核标记
  nowText: any = null; //所选取表格的内容
  row: any = { row: { seen: '' } }; //上一个选取的row记录
    msg: any = null;

  mydata: any = null;
  statistics_result: any = [];
  area_list: any = [];
  source_list: any = [];
  category_list: any = [];
  category_selected: any = null;

  source_selected: any = null;
  startTime_selected: any = null;
  endTime_selected: any = null;

  url: any = null;
  multipleSelected_url: any = [];
  // promises = []
  tempdata: any = null;
  pictLoading = false;
  statistics_result_Buoy: any = null;
  statistics_result_Ship: any = null;
  statistics_result_Station: any = null;
  loadingBtn = false;

  code: any = null;


  // lifecycle hook
  mounted() {
    this.loadMission();

    //this.mission_list=getAllSource();

    //alert(this.mission_list.code);
  }

  //computed
  get computedTest() {
    return null;
  }

  loadMission() {
    let url = `${host}/Mission/getMissionList`;

    axios
      .get(url, {
        params: this.mission_list,
      })
      .then((res) => {
        //alert(res.data.code);
        if (res.data.code == 'OK') {
          this.mission_list = res.data.result;
        } else {
          alert(res.data.msg);
        }
      })
      .catch((err) => {
        alert('err');
      });
  }

  loadTable() {
    let url = `${host}/Relation/getMissionDaily`;
    axios.post(url, this.mission_selected).then((res) => {
      if (res.data.code == 'OK') {
        this.daily_data = res.data.result; //获取结果
        this.creatTime = this.daily_data.date; //更新日期
        this.dataList = this.daily_data.seList; //获取数据列表
        this.getTableLabels(); //获取表格列名称
        this.getTableData(); //获取表格内容
      }
    });
  }
  getTableLabels() {
    let elementNum = 0;
    let newElement = 1;
    this.elementList = [];
    //let eidNow:String[];
    //查找有几种要素
    for (var i = 0; i < this.dataList.length; i++) {
      //alert(this.dataList[i].elementInfo.eid)
      //最新eid
      var eidNow = this.dataList[i].elementInfo;
      newElement = 1; //认为有新的要素
      //遍历已知要素号，若有相同的则认为不是新要素
      this.elementList.forEach((element: { eid: any }) => {
        if (element.eid == eidNow.eid) newElement = 0;
      });
      //若是新要素则加入
      if (newElement == 1) {
        this.elementList.push(this.dataList[i].elementInfo);
      }
    }
    //将标题信息及相关数据绑入table_labels
    this.table_labels = []; //清空
    //this.table_labels.push({      prop: 'sName',      label: '站点名称',      width: '100',      eid: '-1',    }); //加入站点名一列
    //为不同要素制作相应的表头
    for (var i = 0; i < this.elementList.length; i++) {
      var labels = this.elementList[i].displayLabels.split(';');
      //为每个要素不同的表头名进行处理
      for (var j = 0; j < labels.length; j++) {
        let lableTemp = {
          prop: this.elementList[i].eAbbreviation + (j + 1), //按eABB+j的方式命名
          label: labels[j], //写入表头
          width: '100',
          eid: this.elementList[i].eid, //记录所属的eid
        };
        //将写好的表头信息加入表头列表中
        this.table_labels.push(lableTemp);
      }
    }
  }

  getTableData() {     
    for (var i = 0; i < this.dataList.length; i++) {
      this.dataList[i].sName = this.dataList[i].stationInfo.sName; //赋予站点名称
      //this.dataList[i].seen ='';
      let dl = this.dataList[i].datasList;
      for (var j = 0; j < dl.length; j++) {
        this.dataList[i][
          this.dataList[i].elementInfo.eAbbreviation + dl[j].prescriptionNum
        ] = dl[j].data;//获取要素简称和时效作为表格prop
        if(dl[j].data==dl[j].objData) this.dataList[i].equalObj=true;//是否有修改标记
        this.dataList[i].warnningLevel=0;//警报级别（暂未做）
      }      
    }
    //this.table_data=this.dataList;
    //debugger;
    // this.table_data = [
    //   {
    //     sName: 'Chengdu1',
    //     wav1: 'Chengdu',
    //     wav2: '成都',
    //     wav3: 'wav3',
    //     seen: '',
    //     test:{a:1,
    //     b:2,}
    //   },
    //   {
    //     sName: 'Chengdu2',
    //     wav1: 'Shenzhen',
    //     wav2: '深圳',
    //     wav3: 'wav3',
    //     seen: '',
    //   },
    //   {
    //     sName: 'Chengdu3',
    //     wav1: 'Guangzhou',
    //     wav2: '广州',
    //     wav3: 'wav3',
    //     seen: '',
    //   },
    //   {
    //     sName: 'Chengdu4',
    //     wav1: 'Dalian',
    //     wav2: '大连',
    //     wav3: 'wav3',
    //     seen: '',
    //   },
    // ];
  }
  loseFcous(index: any, row: any,column: any) {
    //debugger;
    //alert('lose' + index);
    //row.seen = '';
    this.checked = row.sName+'的' +column.property + '数据已保存';
  }
//   cellClick(row, column) {
//     this.row.seen = '';
//     row.seen = column.property;
//     this.nowText = row[column.property]; //获取当前文本
//     this.checked = 'click' + row.seen + column;
//     this.row = row;
//   }
//   getColumnName(column: any) {
//     return column.property;
//   }
//   getTableText(row: any, column: any) {
//     return row[column.property];
//   }
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
            }
          }
        }
      }
    }
  }
}
</style>