<template>
  <div class="center">
  <div class="center-welcome">
    <div class="title">
      <h1>WELCOME TO</h1><div class="important"><h1> RTDB</h1></div>
    </div>
    <div class="content">
      <h3>To dowonload data, please read data introduction before  </h3>
    </div>
  </div>
<div class="center-header ">

            <div class="card-form center-header-left">
                <div class="detail-card">
                    <div class="thumbnail">
                        <img src="/images/logo/BUOY_LOGO.png">
                        <div class="caption">
                            <h3>Buoy</h3>
                            <p>North Sea Buoy hourly data including elements: buoy id with longitude and latitude; year, month, date, hour (UTC); sea level air pressure, sea level air temperature; significant wave height, maximum wave height; sea surface temperature; data file format .csv.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="card-form center-header-mid">
                <div class="detail-card">
                    <div class="thumbnail">
                        <img src="/images/logo/STATION_LOGO.png">
                        <div class="caption">
                            <h3>Station</h3>
                            <p>14 stations hourly data including elements: station name with longitude and latitude; year, month, date, hour (UTC); wind direction, wind speed; sea level air pressure, sea level air temperature; wave period, significant wave height, maximum wave height; swell direction, swell period, swell height; sea surface temperature; data file format .csv.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="card-form center-header-right">
                <div class="detail-card">
                    <div class="thumbnail">
                        <img src="/images/logo/SHIP_LOGO.png">
                        <div class="caption">
                            <h3>Volunteer Ship</h3>
                            <p>Chinese volunteer ships hourly data in the area of 15-42'N and 105-130'E including elements: ship name with longitude and latitude; year, month, date, hour (UTC); wind direction, wind speed; sea level air pressure, sea level air temperature; data file format .csv.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
                <div class="center-center ">
                  <div class="center-center-left blue-green-bg">
                <!-- 统计的title -->
                <div class="statistics-title">STATISTICS</div><br>
                <!-- 下面的具体内容 -->
                <div class="statistics-form">
                    <!-- station -->
                    <div  class="card-form">
                        <div class="title">{{statistics_result_Buoy.name}}</div>
                        <ul class="num-box">
                            <li>
                              <p class="num">
                                <span id="num">
                                  
                                  <!-- {{statistics_result[0].fileNumber}} -->
                                  <countTo :startVal='0' :endVal='statistics_result_Buoy.fileNumber' :duration='6000' :autoplay=true></countTo>
                                  
                                  </span>
                              </p>
                             <p class="text">
                                <span id="fileNumber_text">file numbers</span>
                              </p>
                            </li>
                            <li>
                              <p class="num">
                                <span id="size_num"><countTo :startVal='0' :endVal='statistics_result_Buoy.size' :duration='6000' :autoplay=true></countTo></span>
                              </p>
                              <p class="text">
                                <span id="size_text">total size(KB)</span>
                              </p>
                            </li>
                        </ul>
                        <div class="body">
                            <ul>
                              <!-- v-for循环没有成功,先遗留 -->
                                <!-- <li>file numbers:<span>{{statistics_result[0].fileNumber}}</span></li>
                                <li>total size(Byte): <span>{{statistics_result[0].size}}</span></li> -->
                                <li>begin time: <span>{{statistics_result_Buoy.beginTime}}</span></li>
                                <li>end time: <span>{{statistics_result_Buoy.endTime}}</span></li>
                                <li>format: xml</li>
                            </ul>
                        </div>
                    </div>
            
                    <div class="card-form">
                        <div class="title">{{statistics_result_Station.name}}</div>
                            <ul class="num-box">
                            <li>
                              <p class="num">
                                <span id="num"><countTo :startVal='0' :endVal='statistics_result_Station.fileNumber' :duration='6000' :autoplay=true></countTo></span>
                              </p>
                             <p class="text">
                                <span id="fileNumber_text">file numbers</span>
                              </p>
                            </li>
                            <li>
                              <p class="num">
                                <span id="size_num"><countTo :startVal='0' :endVal='statistics_result_Station.size' :duration='6000' :autoplay=true></countTo></span>
                              </p>
                              <p class="text">
                                <span id="size_text">total size(KB)</span>
                              </p>
                            </li>
                        </ul>
                        <div class="body">
                            <ul>
                                <!-- <li>file numbers:<span>{{statistics_result[1].fileNumber}}</span></li>
                                <li>total size(Byte): <span>{{statistics_result[1].size}}</span></li> -->
                                <li>begin time: <span>{{statistics_result_Station.beginTime}}</span></li>
                                <li>end time: <span>{{statistics_result_Station.endTime}}</span></li>
                                <li>format: txt</li>
                            </ul>
                        </div>
                    </div> 
                    <!-- ship -->
                    <div class="card-form">
                        <div class="title">{{statistics_result_Ship.name}}</div>
                                           <ul class="num-box">
                            <li>
                              <p class="num">
                                <span id="num"><countTo :startVal='0' :endVal='statistics_result_Ship.fileNumber' :duration='6000' :autoplay=true></countTo></span>
                              </p>
                             <p class="text">
                                <span id="fileNumber_text">file numbers</span>
                              </p>
                            </li>
                            <li>
                              <p class="num">
                                <span id="size_num"><countTo :startVal='0' :endVal='statistics_result_Ship.size' :duration='6000' :autoplay=true></countTo></span>
                              </p>
                              <p class="text">
                                <span id="size_text">total size(KB)</span>
                              </p>
                            </li>
                        </ul>
                        <div class="body">
                            <ul>
                              <!-- <li>file numbers:<span>{{statistics_result[2].fileNumber}}</span></li>
                                <li>total size(Byte): <span>{{statistics_result[2].size}}</span></li> -->
                                <li>begin time: <span>{{statistics_result_Ship.beginTime}}</span></li>
                                <li>end time: <span>{{statistics_result_Ship.endTime}}</span></li>
                                <li>format: -</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="center-center-right default-bg">
                <div class="search-title">
                  Choose data category, observation area, product period, start time and end time from the following list to start downloading your chosen observation data.
                    <div class="search-subtitle">
                     Please note maximum 31 days for downloading can be chosen at one time search.
                </div>
                </div><br>
                
                <!-- 搜索条件form -->
                <el-form :model="search_form">
                    
                        <el-form-item >
                            <el-select  v-model="category_selected" value-key="id" placeholder="select category">
                              <el-option v-for="item in category_list" :key="item.id" :label="item.name" :value="item.id"></el-option>
                              
                            </el-select>
                        </el-form-item>
                    
                   
                        <el-form-item >
                            <el-select v-model="area_selected" value-key="id"  placeholder="select area">
                              <el-option v-for="item in area_list" :key="item.id" :label="item.name" :value="item.id"></el-option>
                            </el-select>
                        </el-form-item>
                    
                    
                        <el-form-item >
                            <el-select v-model="source_selected" value-key="id" placeholder="select source">
                              <el-option v-for="item in source_list" :key="item.id" :label="item.name" :value="item.id"></el-option>
                            </el-select>
                        </el-form-item>
                    
                    
                        <el-form-item>
                            <el-date-picker class="time_select" v-model="startTime_selected" type="datetime" placeholder="select start time(UTC)" @change="dataSearch" value-format="yyyy-MM-dd HH" format="yyyy-MM-dd HH"></el-date-picker>
                        </el-form-item>
                    
                    
                        <el-form-item>
                         <el-date-picker class="time_select" v-model="endTime_selected" type="datetime" placeholder="select end time(UTC)" @change="dataSearch" value-format="yyyy-MM-dd HH" format="yyyy-MM-dd HH"></el-date-picker>
                        </el-form-item>
                    <el-form-item>
                    <!-- <div class="btn"> -->
                        <el-button type="success" class="btn btn-primary col-md-6" @click="submitForm">Search Data</el-button>
                    <!-- </div> -->
                    </el-form-item>
                </el-form>
            </div>
          </div>
            <!-- 查询结果 -->
            <div class="center-footer">
                <div class="center-footer-card-header">
                RESULTS
            </div>
                 <div class="center-footer-card-body">
                   <!-- 表头内容 -->
                   <!-- <div class="table table-striped table-bordered"> -->
                  <el-table class="result_table" :data="results_data" ref="multipleTable" @selection-change="handleSelectionChange" v-loading = this.pictLoading
                  element-loading-background = "rgba(0, 0, 0, 0.5)"
                 element-loading-text = "Loading..........."
                  element-loading-spinner = "el-icon-loading">
                    <el-table-column type="selection"></el-table-column>
                    <el-table-column type="index"  label="Index" width="80px"></el-table-column>
                    <el-table-column prop="id" v-if="show" label="Id" ></el-table-column>
                    <el-table-column prop="name" label="Filename" ></el-table-column>
                    <el-table-column prop="date" label="Date" ></el-table-column>
                    <el-table-column prop="category" label="Category" ></el-table-column>
                    <el-table-column prop="area" label="Area" ></el-table-column>
                    <el-table-column prop="source" label="Source" ></el-table-column>
                    <el-table-column prop="size" label="Size(Byte)" ></el-table-column>
                    <!-- <el-table-column prop="url" label="url" ></el-table-column> -->
                    <el-table-column label="Action">
              　　　　<template slot-scope="scope">
              　　　　　　<el-button type="info" class="btn_show"  @click="showData('/'+ scope.row.url)">show data</el-button>
              <!-- <a :href="'http://localhost:8080/'+ scope.row.url">show data</a> -->
              　　　　</template>
　                　</el-table-column>
                  </el-table>
                 <div class="result_table_tips">default load 20 data up to date</div>
                  <el-button type="success" class="btn_download" @click="download()" :loading=this.loadingBtn>
		              {{loadingBtn?'Loading....':'Download Data'}}</el-button>

                 <div>
                 </div>
            <!-- </div> -->
        </div>
  </div>
</div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from "vue-property-decorator";
import axios from 'axios';
import JSZip from 'jszip';
import FileSaver from 'file-saver'
import moment from 'moment'
import countTo from 'vue-count-to';
const host = 'http://localhost:8083'
@Component({
  components:{countTo}
})
export default class DataView extends Vue {
  
  mydata: any = null;
  statistics_result: any = [];
  area_list : any = [];
  source_list: any =[];
  category_list:any=[];
  category_selected : any = null;
  area_selected : any = null;
  source_selected :any = null;
  startTime_selected :any = null; 
  endTime_selected :any = null;
  results_data: any =[];
  url:any = null;
  multipleSelected_url: any = []
  // promises = []
  tempdata : any=null
  pictLoading = false
  statistics_result_Buoy: any = null
  statistics_result_Ship: any = null;
  statistics_result_Station: any = null;
  loadingBtn = false
// lifecycle hook
mounted() {
      getStatisticsByCategory().then((res:any)=>{
      if(res.status ===200){
        this.statistics_result = res.data;
        
        for(var i = 0;i<this.statistics_result.length;i++){
          //1.调整前端显示格式
          if(null != this.statistics_result[i].size){
            //换算为KB
            this.statistics_result[i].size = Math.round(this.statistics_result[i].size/1024)
          }
          if(null != this.statistics_result[i].beginTime){
            this.statistics_result[i].beginTime = moment(this.statistics_result[i].beginTime).format('YYYY-MM-DD HH:mm:ss')
          }
          if(null != this.statistics_result[i].endTime){
            this.statistics_result[i].endTime = moment(this.statistics_result[i].endTime).format('YYYY-MM-DD HH:mm:ss')
          }
          //2.赋值
          if("BUOY" == this.statistics_result[i].name){
            this.statistics_result_Buoy = this.statistics_result[i]
          }
            if("SHIP" == this.statistics_result[i].name){
            this.statistics_result_Ship = this.statistics_result[i]
          }
            if("STATION" == this.statistics_result[i].name){
            this.statistics_result_Station = this.statistics_result[i]
          }
        }

      }else{
        alert('Data statistics failed');
      }
    })
    this.reloadData()
    //获取全部区域
    getAllArea().then((res:any)=>{
      if(res.status ===200){
          this.area_list = res.data;
          
      }else{
        alert('Area request failed');
      }
    })
       //获取全部数据类型
    getAllCategory().then((res:any)=>{
      if(res.status ===200){
          this.category_list = res.data;
          
      }else{
        alert('Category request failed');
      }
    })
    //获取全部数据源
    getAllSource().then((res:any)=>{
      if(res.status ===200){
          this.source_list = res.data;
      }else{
        alert('Source request failed')
      }
    })

    

  }

  //computed
  get computedTest() {
    return null;
  }

  //methods
  //预加载数据（10天)，由最近10天改为最近20条，方法暂时不删
  reloadData(){
    this.pictLoading = true
      // var now = new Date() 
      // var last_10_day = now.getTime() - 864000000
      // var startTime = last_10_day
      // var endTime = now.getTime()
      this.reload()
      this.pictLoading = false
  }
  //批量打包下载
  // 批量下载
  async downloadByZip(){
    if(this.multipleSelected_url == null||this.multipleSelected_url.length<1){
      alert('please select data first')
      return
    }
    //改变按钮样式
    this.loadingBtn = true
    var data_url = new Array()
    for(var i=0; i< this.multipleSelected_url.length;i++){
      var url = '/'+ this.multipleSelected_url[i].url.replace(/\\/g,'/');
      data_url.push(url)
    }
    // const data_url = ['/STATION/XMD/2019/08/18/081818.XMD', '/STATION/ZFD/2019/08/18/081815.ZFD']
    var file_name =''
    var data = '1'
    
    var promises = new Array();
      // 初始化一个zip打包对象
    var zip = new JSZip();
    // zip.file("Hello.txt", "Hello World\n");
    
   await data_url.forEach(item =>{
    const promise =  this.getFile(item).then(data=>{
      //替换反斜杠
        item = item.replace(/\\/g,'/');
        const arr_name = item.split('/')
        //获取文件名
        file_name = arr_name[arr_name.length - 1]
        // alert(data)
        zip.file(file_name, data, {binary: true});
      })
        promises.push(promise)
      })
      Promise.all(promises).then(()=>{
          zip.generateAsync({
          type: "blob"
        }).then((content:any) => { // 生成二进制流
          FileSaver.saveAs(content, "data.zip") // 利用file-saver保存文件
          //改变按钮样式
          this.loadingBtn = false
        })
      })

  }
  //数据预览方法
  showData(showData_url:string){
    showData_url = showData_url.replace(/\\/g,'/');
    var page = window.open(showData_url, 'data','alwaysRaised=yes')
    
    
  }
//下载按钮事件
download(){
 this.downloadByZip()
}

//下载单个文件方法
getFile(url:string){
 return new Promise((resolve, reject) => {
        axios({
          method: 'get',
          url: url,
          responseType:'blob',
            
        }).then((res) => {
           
          // alert('axios' + '             '+ res.data)
          resolve(res.data)
        }).catch(error => {
          reject(error.toString())
        })
      })
    }
  //获取选中的值
  handleSelectionChange(val:any) {
      this.multipleSelected_url = val;               //  this.multipleTable 选中的值
  }
  submitForm(){
  this.pictLoading = true
  let url = `${host}/data/getDataInfoResultsByQuery`
  //绑定数据
  
  let data = {
    'categoryId': this.category_selected ,
    'areaId': this.area_selected,
    'sourceId': this.source_selected,
    'beginTime': this.startTime_selected,
    'endTime': this.endTime_selected
  }
  axios.post(url,data).then(res=>{
    // alert(res.data);
        if(res.data[0].state){
          
          this.results_data = res.data;
          for(var i = 0;i<this.results_data.length;i++){
            this.results_data[i].date = moment(this.results_data[i].date).format('YYYY-MM-DD HH:mm:ss')
          }
          // alert(this.results_data[0].url)
        this.pictLoading = false
        }else{
          alert(res.data[0].msg)
          this.pictLoading = false
        }
        
  }).catch(err=>{
    alert('error, please contact administrator');
    this.pictLoading = false
  });
  // 
  }

//预加载数据(20条数据)
  reload(){
  let url = `${host}/data/getDataInfoResultsByQuery`
  //绑定数据
  
  let data = {
    'page': 0,
    'size': 20
  }
  axios.post(url,data).then(res=>{
    // alert(res.data);
        if(res.data[0].state){
          this.results_data = res.data;
            for(var i = 0;i<this.results_data.length;i++){
            this.results_data[i].date = moment(this.results_data[i].date).format('YYYY-MM-DD HH:mm:ss')
          }
        }else{
          alert(res.data[0].msg)
        }
         
  }).catch(err=>{
    alert('err');
  });
  }
}

//获取全部海区
const getAllArea = () =>{
  let url = `${host}/data/getAllArea`
  return axios.get(
    url,
    {
      params: []
    }
  )
}

//获取全部数据源
const getAllSource = () => {
  let url = `${host}/data/getAllSource`
  return axios.get(
    url,
    {
      params: []
    }
  )
}


//获取全部数据类型
const getAllCategory = () => {
  let url = `${host}/data/getAllCategory`
  return axios.get(
    url,
    {
      params: []
    }
  )
}

//根据类别获取统计信息
const getStatisticsByCategory =  () => {
  let url = `${host}/data/statistics`
  return axios.get(
    url,
    {
      params: []
    }
  
  )
}

//初始化时间
const initData = () =>{
      var now   = new Date();
      var monthn = now.getUTCMonth()+1;
      var yearn  = now.getUTCFullYear();
      var dayn = now.getUTCDate();
      var h = now.getUTCHours();
      var m = now.getUTCMinutes();
      var s = now.getUTCSeconds
      return yearn+"-"+monthn+"-"+dayn+" "+h+" "+m+" "+s;
}


//查询
const searchData = () =>{


}


</script>
<style scoped lang="less">
@import "../styles/base.less";
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
  // .center-header-welcome{
  //   display: flex;
  //   flex: 1;
  //   flex-direction: row;
  //   .title{
  //     display: flex;
  //     flex: 1;
  //   }
  //   .content{
  //     display: flex;
  //     flex: 1;
  //   }
  // }
  // 横向排列，显示三个说明
  .center-welcome{
    display: flex;
    flex-direction: column;
    align-items:center;
    justify-content:center;
    .title{
      display: flex;
    .important{
      display: flex;
      h1{
      color: #33CCCC;
      white-space: pre;
      }
    }
    }


  }
  .center-header {
    display: flex;
    flex: 1;
    // background: rgb(59, 126, 51);

    // 三种data的缩略图card样式父样式
    .card-form {
      display: flex;
      flex: 1;
      .detail-card {
        display: flex;
        justify-content: center;
        .thumbnail {
          img {
              width: 150px;
              height: 150px;
          }
          h3 {
            text-align: center;
            font-weight: bolder;
            color: #000000;
            // font-family: "Roboto",sans-serif;
          }
          p {
            font-size: 1.2rem;
            font-weight: 400;
            color: #000000;
          }
          width: 60%;
          @margindefault();
        }
      }
    }
  }

  // 中间部分
  // 横向排列
  .center-center {
    display: flex;
    flex: 1;
    flex-direction: column;
    // background: rgb(145, 172, 73);

    // 中间部分的统计信息（station|fub|ship）的一些有序列表
    .center-center-left {
      display: flex;
      flex: 3;
      // background: rgba(39, 216, 216, 0.897);
      @bluebackground();
      flex-direction: column;
      @margindefault();
      @borderradius();
      .statistics-title {
        // color: yellow;
        // font-family: Arial, Helvetica, sans-serif;
        // text-shadow: 2px 2px 10px #000;
        font-size: 3rem;
        @minortitle();
      }

      .statistics-form {
        display: flex;
        flex-direction: row;
        justify-content: center;

        .card-form {
          display: flex;
          flex: 1;
          flex-direction: column;
          // 注意此处为副轴方向
          align-items: center;
          font-family: Arial, Helvetica, sans-serif;
          text-shadow: 2px 2px 10px #000;
          .title {
            font-size: 2rem;
            color: white;
          }
          .num-box{
            column-count:2;
            color: white;
            list-style: none;
            .num{
              font-size: 3rem
            }
            .text{
              font-size: 1.5rem
            }

          }
          .body {
            ul {
              li {
                line-height: 3em;
                color: white;
                font-size: 1rem;
                list-style: none
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
    }

    .center-center-right {
      display: flex;
      flex: 1;
      flex-direction: column;
      // background: rgb(15, 177, 163);
      @graybackground();
      @margindefault();
      @borderradius();
      .search-title {
        // @minortitle();
        text-align:left;
        font-size: 1rem;
        color: black;
        padding-left:40px;
        // font-size: ;
      
      .search-subtitle {
        // @minortitle();
        font-size: 0.8rem;
        color: black;

      }
      }
      .el-form {
        display: flex;
        flex: 1;
        flex-direction: row;
        flex-wrap:wrap;
        align-items: center;
        justify-content: center;
        .el-select {
          display: flex;
          flex: 1;
          justify-content: center;
          font-size: 1.5rem;
          width:250px;
          // border:40px #33CCCC;
          //已取消label
          label {
            // @whitefont();
            color: black;
            font-size: 2rem;
          }
        }
        .time_select{
          width:320px;
          // border: 10px #33CCCC;
        }
        // .btn {
          
          button {
            display: flex;
          justify-content: center;
            @bluebackground();
            border:none;
            // background-color: #2bbbad !important;
            width:250px
          }
        // }
      }
    }
  }

  .center-footer {
    display: flex;
    flex: 2;
    // background: rgb(42, 134, 146);
    @graybackground();
    flex-direction: column;
    @margindefault();
    @borderradius();
    .center-footer-card-header {
      font-size: 2rem;
      color: black;
      //  @minortitle();
    }

    .center-footer-card-body {
      background: white;
      font-size: 1rem;
        color: black; 
        align-self: auto;
        .result_table{
          height: 500px;
          overflow: scroll;
        }
// 修改滚动条样式
    // .table table-striped table-bordered::-webkit-scrollbar-thumb{
    //     border-radius: 2px;
    //     height: 50px;
    //     background: #eee;
    // }
    // .table table-striped table-bordered::-webkit-scrollbar-track{
    //     box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    //     border-radius: 2px;
    //     background: rgba(0,0,0,0.4);
    // }
          .btn_show {
          display: flex;
          // margin:0 auto;
            @bluebackground();
            border:none
            // background-color: #2bbbad !important;
          
        }
        .result_table_tips{
          display: flex;
          justify-content: left;
          font: 1rem;
        }
        .btn_download {
          display: flex;
          justify-content: center;
          margin:0 auto;
          width: 200px;
            @bluebackground();
            border:none
            // background-color: #2bbbad !important;
          
        }
    }
  }
}
</style>
