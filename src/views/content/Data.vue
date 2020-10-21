<template>
  <div class="center">
    <div class="center-header">
      <div class="title">数据订正</div>
      <br />
    </div>
    <div class="center-center">
      <div class="center-center-left" height="1000" width="400">
        <div class="card-form" >
          <!-- <div class="el-input"> -->
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
            height="500"
            width="400"
            @cell-click="cellClick"
          >
            <!--  -->
            <el-table-column fixed="left" prop="sName" label="站点名称"></el-table-column>
            <el-table-column
              v-for="(item, index) in table_labels"
              :key="index"
              :prop="item.prop"
              :width="item.width"
              :label="item.label"
              align="center"
            >
              <template scope="scope">
                <el-input
                  WarningColor="0"
                  v-model="scope.row[scope.column.property]"
                  v-show="scope.row[scope.column.property]!=null"
                  v-if="scope.row.seen===scope.column.property"
                  @focus="getPicture(scope)"
                  @blur="loseFcous(scope.$index, scope.row,scope.column)"
                ></el-input>
                <span
                  :class="{'el-input0':scope.row[scope.column.property+'WL']===0,'el-input1':scope.row[scope.column.property+'WL']===1,'el-input2':scope.row[scope.column.property+'WL']===2,'el-input3':scope.row[scope.column.property+'WL']===3,'el-input4':scope.row[scope.column.property+'WL']===4,'colorDifferent':!scope.row[scope.column.property+'equalObj'],'colorSame':scope.row[scope.column.property+'equalObj']}"
                  v-else
                >{{getTableText(scope.row,scope.column)}}</span>
                <!-- :class="{'el-input0':warnningLevel===0,'el-input1':warnningLevel===1,'el-input2':warnningLevel===2,'el-input3':warnningLevel===3,'el-input4':warnningLevel===4}"
                v-if=""-->
                <!-- -->
              </template>
            </el-table-column>
          </el-table>
          <!-- </div> -->
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
                当前日期:
                <span>{{creatTime}}</span>
              </li>
              <li>
                完成状态:
                <span>{{isOK}}</span>
              </li>
              <li>
                预报员:
                <span>{{forecastName}}</span>
              </li>
              <li>
                审核状态:
                <span>{{isChecked}}</span>
              </li>
              <li>
                审核人:
                <span>{{checkerName}}</span>
              </li>
              <li>
                实时状态信息:
              </li>
              <li>
                <span>{{checked}}</span>
              </li>
              <li>
                <el-button class="buttonType1" type="success" plain @click="confirmCompletion">预报完成</el-button>
              </li>
              <li>
                <el-button class="buttonType1" type="success" plain @click="auditCompletion">审核完成</el-button>
              </li>
              <li>
                <el-button class="buttonType2" type="success" plain @click="auditCancel">取消审核</el-button>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div>
        <img :src="picture">
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
  dataList: any = []; //每日数据 daily_data中的数据列表（之后升级用作表格内容）
  elementList: any = []; //要素信息列表
  //eLableList: String[] = []; //要素标题列表
  checked: any = null; //审核标记
  nowText: any = null; //所选取表格的内容
  row: any = { row: { seen: '' } }; //上一个选取的row记录
  msg: any = null;
  warnningLevel: any = 2;
  checkerName: any = null;
  forecastName: any = null;
  isOK:any='未完成';
  isChecked :any='未审核';
  picture:any= null;// 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANsAAACJCAYAAAClxU28AAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAABVASURBVHhe7Z2Jc1TnlcX5u/hzZjLyMtgGbIy3GDvj3cERsQFjbAyObTLOVCpVnmGSTJxMUkmNF4zZZRCrWCWw9p1W37n3fsv73ntft1731/0s4JyqWyAhtdW/0+f0fa+f2usIgqBahLBBUE1C2CCoJiFsEFSTEDYIqkkIGwTVJIQNgmoSwgZBNQlhg6CahLBBUE1C2CCoJiFsEFSTEDYIqkkIGwTVJIQNgmoSwgZBNan7sK0s0eLoFzR/9XMz1z6nBZ3/pIXrbv5LZ/GGm0N+lm7+dzC/p+VRN3+g5TGeW2bu3Pqjnf+hxm03fzLzg8wXZsb/TCt+/kIrE3+h5sT/mpmU+audv5mZ+jvRwgW+I01zf6D2gt/J6j5sjVlqjn5C0yefpfFvHtSZOPyQmW8foslvH6bJIw/T1Hf/yrOBpo5uoOmjj+jMHHuU5zEzxzfS7PFNNHeC5+Rmmj/5OM2f4jn9BC0M8Xy/hedJnq20dIbn7FM8T9Py8DM8z5o5/xzdOf9Tnud5ttGdC9uocfEFWrn4Is/PaOWyzEs8L+s0R16h5pXXeF6n5uJVe4egtoLfyUoKG419ynOQ5s6+Sj8w9PHDbII1QOHzTB1h+DJsQB6+NUDh85zgUfg8OfgyAv9JWhT4Zxz8wIBzzzJ0MUDg8xThXxL4/+YNMPBfNQbMHrd3CGor+J2s9LCNfqR/Ll3eTRNHHskM4LbTxpO2c/DVgA0BfJ4Qvmu7U8aAhULbLZ7JDIjDt213gediYEAJPg/C1pngd7KSw9a8+YE3oHH9AE0e3VyC79YLhc8zE7bd8ccYPo+FnxnA8L0BEfiFtrtzzsHnEfhh212SEfjGgKYYoPB5ELZqgt/J6knYmjfe479/otMcPUgzp573+7yBL02Xja4WagCPh58ZMHdSxsHnUfhbPHw1gHf5PPyg7XLweWLw3WqBsFUT/E5Wz8Omc+szWji33TSdjlstHHxZLyx8brs4/M005+DbtltUAwz8xbNbbdsJ/GcMfG07ngvGgIYzQOHLLm8MaIoBCFtngt/J6k/Y1IBf0/LIXm65DWrAlKwW1gABrwY4+N4Agb8xg+/WC4F/2sG3bcfw9cD5bAG+TAj/wgvUuCQGFOHbtkPYqgl+J6t/YdM5SI0bn9D08S26XqgBAt8eOBv45bYz8I0BAl/PVPEsatsJfB6F73Z5gS/j4BsDDHwehZ+1XVMM8PCP2TsEtRX8Tlafw2amees/aHboRWOAwncGOPg8xzMDMvibGH5mgMIfysN3u7yBn18tGsFq0UDY0gS/k1VL2HRu/YYWLuygSQbvDeC282eqGL4aoPCztvPweXSPDwzIwS/u8Xzg7OFr2zn4P6OmGDBiVwuErZrgd7LqC5sMH0gvX91HU8c2GgMUvjXAw3+U5nS1MAbk4Idtx+vFkmu74acC+Dw5+FnbudUCYetC8DtZ9YZN5yCtjH5GMye32kt6rAFutWD42nbWAAPfGOAPnMUAgc8Hzg7+Uth252UYfth2wWrRlHEHzQhbNcHvZCWGjffzjuHLfMx7/W9p7sxLmQG6y2fwnQHzp1zbbWbw1gCFv4WW5Po5abso/OcC+DwC377gmcHnQdiqCX4n60cI28dEo78yVyHoXv+WwndnqvS0cAjfrxYBfB4PX9pu2Bjgd/kQvjPAXc7j4dvVAmGrJvidrB8hbALewh89QM2xf6eZE48H8HnsazBzYoDAtwaE8KXtlmS1CNpO4PvXYNrC5xH40nYIWzXB72TVHLag5Rg8je7nz31Ksyefzred7PJsgN/jFf4mC98aMCRtJ/B5tO0cfG47OS3MbdcIDfAXqloDHPwZhK2S4Heyagwbgy+0nMLnj4vw9cCZxzRdZsDC6XCPN2eqPHx7SlinJfxtAXy7WiBs1QS/k1Vv2ErgZT5i+E/ZFz2Ds1QCP2g7WS8W/GrBY+Gbs1Q8vFosu7Y7J23n4FsDLprVwsDn8fCP2jsEtRX8TlZNYWvRcjc/1I9nTjxlX/jM4MvMyXj4Gxl+0HYOvrSdwOe2W3Ztx/Dld54a5wL4do9H2LoU/E5WfWGLtZzAv3mA4W9V+DK512AUfrDLC3zXdkOPe/juNRgDn0fhP8Pwg9VC4T/P8NkAe+2crhYIWzXB72TVEDbXcgK+0HI392Xw3XV0Dj6PwJ9V+DwWvp6lklH4brWQtmP4+kuGGfzcaiHweUL42nYIWzXB72TVE7bSOmHh3xD4+zP4R9wlPRH43HYGvm07D59H4T/J8G3befhB2+XgyyBsHQl+J6vPYXMtV4RvW+7GBwX4DzP8zIDZ4zwM3hngTgmb12A25+HzaqHw9f0qxIAifLNarMhq4eDLi50IWzXB72T1P2ztWk7hf8jwn/S//+SuDpcxL3hmbTfvVwtz4Lzo2s7CN6sFj93j9fWXInwZD5/3eIStmuB3svoYNtdyDnyk5W68H8A3719hfvcpbDsDX0bh27ZT+HzgbOA/YeDzhKuFgc9tJ6/BlODbg+aZ7+wdgtoKfierv2FbreUU/r4cfD1o9vCl7Ri+azuFb9oug8+j8IO2E/jDMfg/tfBlHHw8s1US/E5Wn8JWBN+i5XT22V+jL8DncaeFS/DlBU+G7w0I4X8v8GUEvjVAry4I4Osej2e2jgS/k9WnsAn4WMsJ+BD+Xv27wpd3ZWIDpr7LDPDwbdvNncwMaA2fVwsH37XdeWk7s8dn8M1qgbBVFPxOVh/CFlknoi3H4K/z9wn8Y0/YN/iMw3dtp6eEHXy7WsgLnrJaGPjWALtaxOHzIGydC34nqw9h66DlQviHHzRvgabwjQEKP2g7B18mg89t1wq+Wy0YvlstPHz7YifCVlHwO1k9DluHLafw36epY48z/AdoUgzQPd6MO2h2bafw7aU8Ct++/iIveC751YJHL1SNwX8OYetW8DtZPQ5brOUc+Bj8Pfr3HHx5N90ifGuAuSrctJ2eoXJtJ/CHCvDPCHweWS1K8GUE/jaErargd7J6GLZIy8VO/YYt5+AfZfjfPOBXC9d22dUFBr7f4+2lPL7thjZH4G8N4MuY3+bNwedB2CoKfierR2FjiKu2nIAvtNz1d/nj9xj+Zhr38I0BJfg8Ct+2XR6+HDQzfGeAhZ/f4wX+s7QiBiBsnQt+J6t3YSuCd/DbtZzA5797+N84+DzcdgZ+ZoDAd203fyq/WuhBcxQ+TxS+2eMRtoqC38nqQdjeZ/gMsWXLOfCRlru+O4A/YAxw8Lnt5HRwDr5bLbjt9MXO1eDzRPd4hK1zwe9k9SZsAjIHvmLLXRP4e3LwzUGzga+vv9i2C+HLOPhqgIXvV4u28LntgjNUCFtFwe9kJYbtY265GPyKLefhb/Lwc3t8YbUI4bszVHn4tu3OmLZzF6kqfH8ZD8LWleB3snoQtr0MnyF203LXdunfJ3PwnQEGftZ2Fr41QM9QudVC4LsrC6LweXLwZbVA2DoS/E5WQthmGPavIvA7aDmFv5smv9vI4P8lAj9sO97jc/CDPT6Ar6tFCF9ef/HwzRXhBr5pO4StouB3snoQtvcs/C5a7tpOA//IRvrh6xC+7PIBfG27AD5PGT6Pg28PmpetAQpf36bawG+E8KcP2zsEtRX8TlZi2D4qwC+2XAS8fG0In/9U+F8J/IEIfB4PP1stPHyePHxeLQrwzWrB8LntPHy7x8vbYRM1zX2CWgt+J6tHYWOQHnwMvoCPtNy1dyz8xwz8rzP4slpM5VYLA9+1ncK3e/zC6U0Gvr8iXFaLInxz0GzghwfNL1Bz4s/2TkEtBb+TlRi2AxH4DjzPai13VeDvzOB/JfCdAavDd20Xh7+lIny5skAM+CMXXsPeOagk+J2sHoRtj4XfRctdfVv/PhHAHw/ghwfNU/ruuQH84KDZv9Apw/DdLxdWh7+NmpdfIPrhcwSuleB3shLDtt/CZ5irHSRrywl4C19aTuG/Y+B/aeHrHi8j8F3bmVPCBr4xwMPn1aI9fB4Hn/f4hhw05+AbAwS+vGMuTfzJ3kEoJ/idrD6HzYFv0XI6Av9RC59H9/gMvm+7ruFL08kZqjz83K/Me/gvUvPaL+0dhHKC38nqQdje5R9Y4FdpOQfewec7yn8K/NsBfNN2D9rr5iz8wh7v4Mvk4MseX2o6NiAK37Udw5dfLETYWgt+JystbHyQLJfuGPgCPtZyAj7Wcnwnrxj4498y/K9+4s9QuVPCuaZT+K7tCvBbvPtSDr786oXAlz1e4WengxG2CoLfyepR2BhoNy13ZYf+Of7tIwF8Y0AGP9vjS01nDYjBl3fNXXZnqDz8pxG2bgW/k5UYtn0Mf3cEfouWy4HPw7/1ZQX4wR6vl/BY+O50sIHPo/CfKMA3B80IW5eC38lKCxsfJBv4DLRSywn4oOWuDPLfd+ThqwEM/5sMvlktVoFv3wwmB9+tFiF83eMRto4Fv5OVGLYPGP6uAvwqLWfB64TwfxIcNMfg89jVogw/0nRdweefDSoLfierR2FjoMWWix4kF+H/Qv8M4fvVQuE7A7Km6yV8PUOFsFUT/E5WYtjkFwl3UlOAdtVyDv6GDuCb1cLDlynCH+oU/vMI22qC38lKDxvDLcEvtVwMPoMfecvAP2zg6xkqe8Cch88HzX6tcPCDM1Rdw+fVwv3qBcLWXvA7WT0KG//AHbWcgDfwmyM76PbXD2Xwpen0Mp4HaCIG3x40x+FbA+xVBXH4PCF8e1VBU97AU64qQNjigt/JSgjbNMPfyz/sO3n4pZYT8BH4V7jl2ICFMy/T6D/+KWg6OWi28MPTwZXg2zNUXcPHM1tLwe9kpYWNV4kMftWWc00nLTdIt798QOGPffnP6fDd6WCErfeC38nqUdgYaMctt52Wz79OYww+B5+nKnx/0Mzw9dflC/D9JTxF+Od4j/fw3Q6PsLUV/E5WD8PmwMsI+FjLCXjTcgJ/6dwrGfz/y+DrHt8G/nS38M8ibF0LficrMWx7+Id928CPtpyAL8C34Gnk5wpfwEfhy1UFHcPnKcHnqQpf/jewCFtc8DtZPQobQ63ScvaMlIc/bA6Wpe0E/i0Gbwyw8LntOoMfa7rO4K/wA+nOnTu0srJCzSbeCMgLficrMWxyBTjDF7BVWs7u7gKeRt7sA/z0phP48/PztLi4SMvLywieE/xOVnrYGLKB78C3aDmZoOUM/Jcqw9f/y0kN8Bv8c05PT9PMzAzNzc3ljGg0Gvdv6OB3shLDtjsCP2w5AR9vObq8NuHf4QfI7du3aXx8nCYnJ9UEgS/g72vB72SlhU0PkncyfAbbtuUEfBH+Gx3Br2uHX76yi8bGxrTtpN3u+/XRCX4nKyFsMwxcXmvZxW3HoHPgC/BL4A38Oxe208SJ12ni5Bs0eeoNmjr9Jk0NvUnTQz+nme+308yZ7TR79i2dueFf0Pww79cy53fQwoVf6ixeeJsWL75DSzKXdtLS5V20LDOym1trNzVG3uVVYQ81rr6ns3J1L61ce59Wbnyg05Tf0boh74H4ITXHDtLS7BXd2aGC4Heyug+bvIXzwjm9Xi5bJyItp+DLLUeXXyca+8zeFrT2Bb9TlRA2q+VbbMD+CPj2LQf4d6ngd9dKD5uoMU9063fllmuxuyv4S68RjSJsd6Xgd1fqTdhEcmA59Q8GLga0ajkBH8KX/6MIdFcKfnes3oXNae6MWTF8ywn4SMtdehVhuxcEvyur92ETLY8RXf+QgbdpOYX/a/sN0F0t+F1J/QmbaGWBD4h/U4AfgOdp3vhUX9fAa1n3gOD3qupf2ETNBtHk3yx4HgXv4L9CK9c/oaWlJb0e7b6+FOpeEfxuq/6GzWnuFLcdH0QH4OniK9S49jHNzs7q9WgLCwtqhLsmDRf/3sWC31HVEzaRvD5zbU8A/2VavnKAJiYmaGpqSi+XESPkYlB3ISiu5LiLBb9Lqi9sItnr5bUWbjmBvzSyX69LcxeCykWg0nYI2T0i+J1TvWETyV4//gW33WsKX8BLu8kej7XxHhT89qo/bE5zQ9ScOYqA3S+C3z9i2CDoPhPCBkE1CWGDoJqEsEFQTULYIKgmIWwQVJMQNgiqST0N2/Dgelo/eIgG16+jdesis36Qhu3Xeg0P0vp1A3TIfthaw3y762kwdwOxz7H0NiP//dLkv/fQwDoaKP4ghwb4PpV+aqiizGOiHT/xMPNfvj7nUc6QQzQQ/ps8ntif3NfbCb9trfjaw7C5B36bANiwyZ2PAYqOC6h8f5FY7HOi3Ofl54nA9j+v/ZCFsPVe3YQts67oL4ctfDz48m5zG6x7L2z+zq8etkzSVJGvLarUXuZ7Si3obiswKR5s+TqErdcq+9F6hGnJG4bfVdgKj62ij/dc2ASS+eHlQVyA6KYYNgnRwGDrr3frZQ6MC0kA3urQQBi24HaDr8v/nOWwlX+GwvBtQRUlPii3NocJ+jUdPLMVvDDlbsKU+Zf/760VX3sUNgOh/CCWz9s7Xmif/NcVZJ/JPOdI2AY4WMW2yoct+EdvOo//fDxsxduEupXwHaBBLbdB9isONns2NI+TTp/ZBtmz7LER11rxtSdh02cLfuCW73QQtoIM5PK/6edzoWTZ8GVjQlKEWOWZzd8WPxAGELa+ybHUx4YxK/L44EAOiA/8OLDB6jRsvIzmn+3shN92T4XtEDfXIYXa5kykGwEkoPRj03zRr3MjXx95ZnMfhiDjz2zy9eFtuoDHn9ly/+3S90BVJBydXz5srPDzIhMs8wzo+HoPRZXDlvcnF1jWWvG1D8dsTtJa/Dl+FpIQhjJA4896RVCqNmEzH5vbablGhhqWLV//Ermd8GOn7Pah1SSsyoEKP9YHvvVGStocczm+BdbeR/dYKTyLVXpmWzu+9i1suUBFV4hehc0p+HzOpJgBosLt+KYsqn5T7lZJkAz24trvxvAOA5fjW/Qg9DHqTQWtIV/7EDa5E+6BnQVK/r38AmX1sMVMyyu4PQGc+/pW427H/MzlQhAhbCkqlnBZGV8fVicXNh+64LEljwkXIvXbehR+3n79WvG1x2Ezx2wZsEKgBIT/x/y/ZXt1JEj8ffFnNrmNIDzutr057RTcTs6gouo35V5S5bCJZyUPMn/lNoqFLY8Zd9vh39VP+bo15mvPwgZBUHshbBBUkxA2CKpJCBsE1SSEDYJqEsIGQTUJYYOgmoSwQVBNQtggqCYhbBBUkxA2CKpJCBsE1SSEDYJqEsIGQTUJYYOgmoSwQVBNQtggqCYhbBBUkxA2CKpJCBsE1SSEDYJqEdH/AxDmDYafYE+qAAAAAElFTkSuQmCC';
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
        //this.creatTime = this.daily_data.date; //更新日期
        this.dataList = this.daily_data.seList; //获取数据列表
        this.getTableLabels(); //获取表格列名称
        this.getTableData(); //获取表格内容
        var date = new Date(this.daily_data.date);
        this.creatTime =
          date.getFullYear() +
          '年' +
          date.getMonth() +
          '月' +
          date.getDate() +
          '日';
      } else alert(res.data.msg);
      if(this.daily_data.finished==true)this.isOK='已完成预报';
      else this.isOK='未完成';
      if(this.daily_data.checked==true)this.isChecked='已审核';
      else this.isChecked='未审核';
    });
    this.picture=null;
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
    var forecastName = [];
    var checkerName = [];
    for (var i = 0; i < this.dataList.length; i++) {
      Vue.set(this.dataList[i], 'sName', this.dataList[i].stationInfo.sName); //技术：对于已经创建的实例，vue不允许动态添加响应式property，但可以用此方法向嵌套对象添加响应式property
      //this.dataList[i].sName = this.dataList[i].stationInfo.sName; //赋予站点名称
      //this.dataList[i].seen ='';
      Vue.set(this.dataList[i], 'seen', '');
      let dl = this.dataList[i].datasList;
      for (var j = 0; j < dl.length; j++) {
        Vue.set(
          this.dataList[i],
          this.dataList[i].elementInfo.eAbbreviation + dl[j].prescriptionNum,
          dl[j].data
        );
        // this.dataList[i][
        //   this.dataList[i].elementInfo.eAbbreviation + dl[j].prescriptionNum
        // ] = dl[j].data; //获取要素简称和时效作为表格prop
        if (dl[j].data == dl[j].objData)
          Vue.set(
            this.dataList[i],
            this.dataList[i].elementInfo.eAbbreviation +
              dl[j].prescriptionNum +
              'equalObj',
            true
          );
        //this.dataList[i].equalObj = true; //是否有修改标记
        else
          Vue.set(
            this.dataList[i],
            this.dataList[i].elementInfo.eAbbreviation +
              dl[j].prescriptionNum +
              'equalObj',
            false
          );
        Vue.set(
          this.dataList[i],
          this.dataList[i].elementInfo.eAbbreviation +
            dl[j].prescriptionNum +
            'WL',
          dl[j].warnningLevel
        );
        //this.dataList[i].warnningLevel = 0; //警报级别（暂未做）
      }
      if (this.dataList[i].checker != null)
        checkerName.push(this.dataList[i].checker);
      if (this.dataList[i].forecaster != null)
        forecastName.push(this.dataList[i].forecaster);
    }
    this.checkerName = null;
    this.forecastName = null;
    if (checkerName.length > 0) {
      this.checkerName = [...new Set(checkerName)].join('、');
    }
    if (forecastName.length > 0) {
      this.forecastName = [...new Set(forecastName)].join('、');
    }
  }
  ///用于订正修改之后的保存和信息反馈
  loseFcous(index: any, row: any, column: any) {
    //debugger;
    //alert('lose' + index);
    //row.seen = '';
    this.checked = row.sName + '的' + column.property + '数据已保存';
    //用于修改原dataList的值
    for (var i = 0; i < row.datasList.length; i++) {
      if (
        row.elementInfo.eAbbreviation + row.datasList[i].prescriptionNum ==
        column.property
      )
        row.datasList[i].data = row[column.property];
    }
    //将修改的部分回传
    let url = `${host}/Datas/updateOne`;
    axios.post(url, row).then((res) => {
      if (res.data.code == 'OK') {
      } else alert(res.data.msg);
    });
    this.loadTable();
    //this.getTableData(); //获取表格内容
  }
  cellClick(row: any, column: any) {
    this.row.seen = '';
    row.seen = column.property;
    this.nowText = row[column.property]; //获取当前文本
    this.checked = 'click' + row.seen + column;
    this.row = row;
  }
  //   getColumnName(column: any) {
  //     return column.property;
  //   }
  getTableText(row: any, column: any) {
    return row[column.property];
  }
confirmCompletion(){
  let url = `${host}/Datas/confirmCompletion`;
    axios.post(url, this.mission_selected).then((res) => {
      if (res.data.code == 'OK') {  
        alert("已确认完成");  
        this.isOK='已完成预报';   
      } else alert(res.data.msg);
    });
}
auditCompletion(){
let url = `${host}/Datas/auditCompletion`;
    axios.post(url, this.mission_selected).then((res) => {
      if (res.data.code == 'OK') {  
        alert("已完成审核");  
        this.isChecked='已完成审核';   
      } else alert(res.data.msg);
    });
}
auditCancel(){
let url = `${host}/Datas/auditCancel`;
    axios.post(url, this.mission_selected).then((res) => {
      if (res.data.code == 'OK') {  
        alert("已取消审核");  
        this.isChecked='未审核';   
      } else alert(res.data.msg);
    });
}
getPicture(scope: any){
  //传输参数
  var temp='';
  temp=scope.column.property;
  temp=temp.substring(temp.length-1,temp.length);
  var req={sAbb:scope.row.stationInfo.sAbbreviation,
  eAbb:scope.row.elementInfo.eAbbreviation,
  prescriptionNum:temp,
  };
  
  //请求
  let url = `${host}/photo/get`;
    axios.post(url,req).then((res) => {
      if (res.data.code == 'OK'){
        this.picture=res.data.result.contentType+','+res.data.result.base64Code;
      }      
    });
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
              .buttonType1{
                background-color: rgb(216,191,50);
                color: black;
              }
              .buttonType2{
                background-color: rgb(230,145,34);
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

<style lang="less">
.center-center .center-center-left .card-form .el-input0 {
  font-size: 16px !important;
  background: white; //红色
  //color: black!important;
}

.center-center .center-center-left .card-form .el-input1 {
  font-size: 16px !important;
  background: rgb(255, 199, 199) !important; //红色
  //color: black!important;
}

.center-center .center-center-left .card-form .el-input2 {
  font-size: 16px !important;
  background: rgb(253, 188, 114) !important; //红色important
  //color: black!important;
}

.center-center .center-center-left .card-form .el-input3 {
  font-size: 16px !important;
  background: yellow !important; //红色
  //color: black!important;
}

.center-center .center-center-left .card-form .el-input4 {
  font-size: 16px !important;
  background: rgb(159, 159, 243) !important; //红色
  //color: black!important;
}

.center-center .center-center-left .card-form .colorSame {
  color: black !important;
}
.center-center .center-center-left .card-form .colorDifferent {
  color: red !important;
}
</style>