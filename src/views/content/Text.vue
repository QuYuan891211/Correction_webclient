<template>
  <div class="center">
    <div class="center-header">
      <!-- 统计的title -->
      <div class="title"><h1>文本订正</h1></div>
      <br />
    </div>
    <div class="center-center">
      <div class="center-center-left">
        <div class="card-form">
          
          <div class="body">
            <ul>
              <li>
                <div class="sub-title">文本信息：</div>
                <span><textarea style="display:none;" v-model="textData_id
                "></textarea></span>
              </li>
              <li>
                文本创建日期 :
                <span>{{textData_gmtCreate
                }}</span>
              </li>
              <li>
                文本更新日期 :
                <span>{{textData_gmtModified
                }}</span>
              </li>
              <!-- <li>
                是否已审核 :
                <span><textarea v-model="textData_isok
                "></textarea></span>
              </li> -->
              <li>
                预报员 :
                <span>{{textData_forecaster
                }}</span>
              </li>
              <li>
                审核人 :
                <span>{{textData_checker
                }}</span>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_text_type1"
                @click="getLastDayTextDetail"
              >加载前一日文本内容</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_text_type1"
                @click="saveTextData"
              >预报员保存当日文本</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_text_type2"
                @click="check"
              >审核员确认完成</el-button>
              </li>
              <li>
                <el-button
                type="info"
                class="btn_text_type2"
                @click="cancelCheck"
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
            <el-select v-model="textInfo_selected" value-key="id" @change="loadTextDataAndTextDetail" placeholder="请选择文本模板">
              <el-option
                v-for="item in textInfo_list"
                :key="item.tid"
                :label="item.tName"
                :value="item.tid"
              ></el-option>
            </el-select>
          </el-form-item>

          </div>
          </li>
          <li>
          <div class="title-text">第一天预报文本内容：</div>
          </li>
          <li>

              <el-form-item>
    <textarea class="textArea" v-model="text1
                "></textarea>
</el-form-item>

          </li>
                    <li>
          <div class="title-text">第二天预报文本内容：</div>
          </li>
          <li>

              <el-form-item>
    <textarea class="textArea" v-model="text2
                "></textarea>
</el-form-item>

          </li>
                    <li>
          <div class="title-text">第三天预报文本内容：</div>
          </li>
          <li>

              <el-form-item>
    <textarea class="textArea" v-model="text3
                "></textarea>
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
const host = 'http://localhost:8001';
@Component({
  components: { countTo },
})
export default class TextView extends Vue {
  //绑定下拉框选中的文本模板tid
  textInfo_selected: any = null;
  textData_id: any = null;
  textData_gmtCreate: any = null;
  textData_gmtModified: any = null;
  textData_isok: any = null;
  textData_forecaster: any = null;
  textData_checker: any = null;
  textData_selected_byTid: any =[];
  textDetail_selected_byTextDataId : any = [];
  text1:any = null;
  text2:any = null;
  text3:any = null;
  //绑定后台返回的文本模板
  textInfo_list:any = [];
  text_result: any = null;
  url: any = null;
  tempdata: any = null;
  pictLoading = false;
  loadingBtn = false;
  data1 : any = null;
  isOk :any = null;
  // lifecycle hook
  mounted() {
    //获取全部文本模板（TextInfo）并加载到下拉列表中
    getAllTextInfo().then((res:any)=>
      {
          if(res.status === 200){
            if(res.data.code === 200){
                this.textInfo_list = res.data.data
            }else{
              alert(res.data.message)
            }
          }else{
            alert('获取文本模板失败')
          }
      }
    )

  }

  //computed
  get computedTest() {
    return null;
  }

  loadText(){

  }
  //将选中的文本模板tid传给后台，后台返回TextData数据调整格式后绑定
  async loadTextDataAndTextDetail(){
    
    let did = await this.loadTextData()
    this.loadTextDeatail(did)
    
  }

  //根据TextData id 获取TextDetail
  loadTextDeatail(id:any){
    //将查询的TextDatat的id传给后台，后台返回TextDetail数据后绑定
   let url_getTextDetail =  `${host}/textDetail/getTextDetail`

  let did = {'id':id} 
   axios.post(url_getTextDetail,did).then(res=>{
      
      if(res.status ===200){
        //2. 接受返回的TextDetail对象（List)并封装
         if(res.data.code === 200){
          //  alert(res.data.data.length)
           if(res.data.data.length > 2){
              this.text1 = res.data.data[0].text
             this.text2 = res.data.data[1].text
             this.text3 = res.data.data[2].text
           }else{
             alert('预报文本不足三天预报内容，请联系管理员')
           }

         }else{
           alert(res.data.message)
         }
      }else{
        alert('未知错误，请联系系统管理员')
      }
   }).catch(err=>{
     alert(err.toString())
   });
  }
  //根据Tid获取TextData
  loadTextData(){
    return new Promise((resolve,reject)=>{
        //1. 将选中的模板tid发到后台
    let url =  `${host}/textData/textData`
    let data = {'id':this.textInfo_selected}
     axios.post(url,data).then(res=>{
      // alert('发送请求')
      if(res.status ===200){
        //2. 接受返回的TextData对象（只有1个或null)
         if(res.data.code === 200){
         this.textData_selected_byTid = res.data.data[0]
         this.textData_id = this.textData_selected_byTid.id
         this.textData_checker = this.textData_selected_byTid.checker
         this.textData_forecaster = this.textData_selected_byTid.forecaster
        //3. 调整日期格式
        if(null != this.textData_selected_byTid && null != this.textData_selected_byTid.gmtCreate){
          this.textData_gmtCreate = moment(this.textData_selected_byTid.gmtCreate).format('YYYY-MM-DD HH:mm:ss')
        }
        if(null != this.textData_selected_byTid && null != this.textData_selected_byTid.gmtModified){
          this.textData_gmtModified= moment(this.textData_selected_byTid.gmtModified).format('YYYY-MM-DD HH:mm:ss')
        }
        resolve(this.textData_id)
         }else{
           reject(res.data.message)
         }
      }else{
        reject('发生未知错，误请联系系统管理员')
      }
    }).catch(err=>{
      reject(err.toString())
    });


    })
        
  }
  //获取前一天的文本内容
  getLastDayTextDetail(){
    let url = `${host}/textDetail/lastDay`
    let data = {'id':this.textInfo_selected} 
         axios.post(url,data).then(res=>{
      // alert('发送请求')
      if(res.status ===200){
        //2. 接受返回的TextData对象（只有1个或null)
         if(res.data.code === 200){
          if(res.data.data.length > 2){
              this.text1 = res.data.data[0].text
             this.text2 = res.data.data[1].text
             this.text3 = res.data.data[2].text
           }else{
             alert('预报文本不足三天预报内容，请联系管理员')
           }
         }else{
           alert(res.data.message)
         }
      }else{
        alert('发生未知错，误请联系系统管理员')
      }
    }).catch(err=>{
      alert(err.toString())
    });
  }
//保存当日预报的文本TEXTData和文本内容TextDetail
saveTextData(){
  
  let url = `${host}/textData/checkByForecaster`
    var textDetail_list = [this.text1,this.text2,this.text3]
    let data = {
      'id':this.textData_id,
      'textDetailList':textDetail_list
      } 
         axios.post(url,data).then(res=>{
      // alert('发送请求')
      if(res.status ===200){
        //2. 接受返回的TextData对象（只有1个或null)
         if(res.data.code === 200){
           alert('保存成功')
           //刷新页面的TextData信息
           this.loadTextData()
         }else{
           alert(res.data.message)
         }
      }else{
        alert('发生未知错，误请联系系统管理员')
      }
    }).catch(err=>{
      alert(err.toString())
    });

}

check(){
  let url = `${host}/textData/lastCheck`
  let data = {
      'id':this.textData_id,
      } 
  axios.post(url,data).then(res=>{
          if(res.status ===200){
        //2. 接受返回的TextData对象（只有1个或null)
         if(res.data.code === 200){
          alert('审核员取消确认成功')
          //刷新页面的TextData信息
          this.loadTextData()
         }else{
           alert(res.data.message)
         }
      }else{
        alert('发生未知错，误请联系系统管理员')
      }
  })

}
cancelCheck(){
  let url = `${host}/textData/cancelLastCheck`
  let data = {
      'id':this.textData_id,
      } 
  axios.post(url,data).then(res=>{
          if(res.status ===200){
        //2. 接受返回的TextData对象（只有1个或null)
         if(res.data.code === 200){
          alert('审核员确认成功')
          //刷新页面的TextData信息
          this.loadTextData()
         }else{
           alert(res.data.message)
         }
      }else{
        alert('发生未知错，误请联系系统管理员')
      }
  })

}
  
// //保存文本内容
// saveTextDetail(id:any){
//   let url = `${host}/textDetail/saveTextDetail`
//   alert(id)
//   var text_list = [this.text1,this.text2,this.text3]
//   let data = {'id':id,
//   'text1':this.text1,
//   'text2':this.text2,
//   't'
  
//   }
//   axios.post(url,data).then(res=>{
//       // alert('发送请求')
//       if(res.status ===200){
//         //2. 接受返回的TextData对象（只有1个或null)
//          if(res.data.code === 200){
//           alert('成功存入当天文本内容')
//          }else{
//            alert(res.data.message)
//          }
//       }else{
//         alert('发生未知错，误请联系系统管理员')
//       }
//     }).catch(err=>{
//       alert(err.toString())
//     });
// }


//保存
}
 //methods
  ///获取全部文本模板（TextInfo）
   const getAllTextInfo = ()=>{
     let url = `${host}/textInfo/textInfoListByDepartment`
     return axios.get(
       url,
       {
         params:[]
       }
     )
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
//浅黄按钮背景
@lightyellowbackground: {
  background: rgb(216, 191, 50);
};
//深黄按钮背景
@darkyellowbackground: {
  background: rgb(230, 145, 34);
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
  margin-left: 200px;
  margin-right: 200px;
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
      font-size: 8rem;
      @minortitle();
    }
  }

  .center-center {
    display: flex;
    flex-direction: row;
    // justify-content: space-around;
    .center-center-left {
      display: flex;
      flex-direction: column;
      margin-left: 50px;
      .card-form {
        display: flex;
        flex: 1;
        flex-direction: column;
        // 注意此处为副轴方
        font-family: Arial, Helvetica, sans-serif;
        text-shadow: 2px 2px 10px #000;
        .body {
          ul {
            li {
              line-height: 3em;
              color: white;
              font-size: 1.5rem;
              list-style: none;
              text-align:left;
              .btn_text_type1{
                color: white;
                font-size: 1.5rem;
                width: 270px;
                border:none;
                @lightyellowbackground()

              }
              .btn_text_type2{
                color: white;
                font-size: 1.5rem;
                width: 270px;
                border:none;
                @darkyellowbackground()

              }
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
      // justify-content: flex-start;
      margin-left: 150px;
      .content-form {
        display: flex;
        flex: 1;
        flex-direction: column;
        // 注意此处为副轴方
        font-family: Arial, Helvetica, sans-serif;
        text-shadow: 2px 2px 10px #000;
          ul {
            li {
              text-align:left;
              line-height: 3em;
              color: white;
              font-size: 1.5rem;
              list-style: none;
              .textArea{
                width: 800px;
                overflow-x:hidden;
              }
            }
          }
        
      }
    }
  }
}
</style>
