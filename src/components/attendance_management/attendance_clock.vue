<!--打卡记录-->
<template>
  <div class="w">
    <div class="head">
      <!--选择开始日期和结束日期-->
      <b style="font-size: 18px;margin-left:10px;margin-right: 10px">打卡时间</b>
      <el-date-picker
          v-model="clockTime"
          type="daterange"
          unlink-panels
          range-separator="TO"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :shortcuts="shortcuts"
          value-format="YYYY-MM-DD"
      >
      </el-date-picker>
      <!--    全部部门-->
      <b style="font-size: 18px;margin-left:25px;margin-right: 10px">部门名称</b>
      <el-select size="small" v-model="pageInfo.optionsDeptId" clearable placeholder="选择部门" >
        <el-option
            v-for="item in optionsDept"
            :key="item.deptId"
            :label="item.deptName"
            :value="item.deptId"
        >
        </el-option>
      </el-select>
      <!--搜索框-->
      <b style="font-size: 18px;margin-left:25px;margin-right: 10px">员工名称</b>
      <el-input size="small" v-model="pageInfo.staffName" placeholder="请输入员工名称" style="width:150px;">
      </el-input>

      <!--查询按钮-->
      <el-button @click="about()" style="background-color: #ffffff;border-radius: 30%; margin-left: 20px" size="small">
        <el-icon><i-search />

        </el-icon>
      </el-button>

    </div>
    <div class="head-export">
      <!--导出导入-->
      <el-button type="warning" plain size="small" @click="derive()">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>
      <el-upload
          class="upload-demo"
          action
          :on-change="channel"
          accept=".xls, .xlsx"
          :auto-upload="false"
          :show-file-list="false"
      >
        <el-button type="success" plain size="small">
          <i class="iconfont">&#xe645;</i>
          导入
        </el-button>
      </el-upload>
    </div>
    <!--    表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">
        <el-table-column prop="staffName" label="员工名称"/>
        <el-table-column prop="deptName" label="部门名称"/>
        <el-table-column prop="mornClock" label="早上打卡时间"/>
        <el-table-column prop="afternoonClock" label="下午打卡时间"/>
        <el-table-column prop="updatedTime" label="记录时间"/>
        <el-table-column prop="operate" label="操作">
          <template #default="scope">
            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through1(scope.row)"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon><i-delete /></el-icon> 删除
                </el-button>
              </template>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!--分页-->
    <div class="demo-pagination-block">
      <el-pagination
          v-model:currentPage="pageInfo.currenPage"
          :page-sizes="[3, 5, 10, 50]"
          v-model:page-size="pageInfo.pagesize"
          :default-page-size="pageInfo.pagesize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="pageInfo.total"
          :pager-count="5"
          background
          @size-change="about"
          @current-change="about"
      >
      </el-pagination>
    </div>

  </div>
</template>

<script lang="ts">
import {ref, defineComponent} from "vue";
import {ElMessage, ElMessageBox} from "element-plus";
import {export_json_to_excel} from '/src/excal/Export2Excel.js'
import XLSX from "xlsx";

export default {
  data() {
    return {
      // 分页
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
        // 员工名称
        staffName:"",
        // 部门下拉框值
        optionsDeptId:"",

        // 开始时间
        clockTimeStart:"",
        //结束时间
        clockTimeEnd:""

      },
      // 打卡时间
      clockTime:[],
      //时间选择
      shortcuts: [
        {
          text: "最近一周",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
            return [start, end];
          },
        },
        {
          text: "最近一个月",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
            return [start, end];
          },
        },
        {
          text: "最近三个月",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
            return [start, end];
          },
        },
      ],

      //查询所有部门名称,ID -- 部门下拉框数据
      optionsDept: [],
      //打卡记录数据
      tableData: [],

      value1: "", //日期
      value: ref(""), //选择
    };
  },
  methods: {
    // 点击删除确认按钮触发
    through1(row) {
      /*alert(row.clockRecordId)*/
      this.deletedk(row);
    },
    //分页查询
    about(){
      // 首先清空
      this.pageInfo.clockTimeStart=""
      this.pageInfo.clockTimeEnd=""
      if(this.clockTime != ""){ // 如果选择的打卡时间不为空
        this.pageInfo.clockTimeStart=this.clockTime[0]
        this.pageInfo.clockTimeEnd=this.clockTime[1]
      }

      this.axios
          .get("http://localhost:8010/provider/clockVo/punchcard",{params:this.pageInfo})
          .then((response)=>{
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error){
            console.log(error);
          })
    },

    // 查询所有部门 赋值上下拉框
    selectAllDept(){
      this.axios
          .get("http://localhost:8010/provider/clockVo/selectAllDept")
          .then((response)=>{
            console.log(response);
            this.optionsDept=response.data.data;
          })
          .catch(function (error){
            console.log(error);
          })
    },

    //删除
    deletedk(row){
      this.axios
          .delete("http://localhost:8010/provider/clockRecord/delete/" + row.clockRecordId)
          .then((response)=>{
            console.log(response)
            if (response.data.data === "成功"){
              ElMessage.success("删除成功");
              this.about() //删完在查一次
            }else {
              ElMessage.error("删除失败")
            }
          }).catch(function (error){
        console.log(error);
      })
    },


    // 点击导出操作
    derive() {
      ElMessageBox.confirm(
          '此操作将导出excel文件, 是否继续?',
          '提示',
          {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
          }
      ).then(() => {
        this.deriveExcel();
      }).catch(() => {
        ElMessage({
          type: 'info',
          message: '取消成功',
        })
      })
    },
    // 导出方法
    deriveExcel() {
      var _this = this;
      let tHeader = ["员工名称", "部门名称", "早上打卡时间", "下午打卡时间", "记录时间"]; // 导出的表头名
      let filterVal = ["staffName", "deptName", "mornClock", "afternoonClock", "updatedTime"];//导出其prop属性
      ElMessageBox.prompt('请输入文件名', '提示', {
        confirmButtonText: '生成',
        cancelButtonText: '取消',
      }).then(({value}) => {
        let data = _this.formatJson(filterVal, _this.tableData);
        export_json_to_excel(tHeader, data, value);
        ElMessage({
          type: 'success',
          message: `生成成功`,
        })
      })
          .catch(() => {
            ElMessage({
              type: 'info',
              message: '取消成功',
            })
          })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map((v) => filterVal.map((j) => v[j]));
    },
    // 导入方法
    channel(obj){
      let _this = this;
      // 通过DOM取文件数据
      this.file = obj.raw;
      var rABS = false; //是否将文件读取为二进制字符串
      var f = this.file;
      var reader = new FileReader();
      FileReader.prototype.readAsBinaryString = function (f) {
        var binary = "";
        var rABS = false; //是否将文件读取为二进制字符串
        var pt = this;
        var wb; //读取完成的数据
        var outdata;
        var reader = new FileReader();
        reader.onload = function (e) {
          var bytes = new Uint8Array(reader.result);
          var length = bytes.byteLength;
          for (var i = 0; i < length; i++) {
            binary += String.fromCharCode(bytes[i]);
          }
          if (rABS) {
            wb = XLSX.read(btoa(fixdata(binary)), { //手动转化
              type: 'base64'
            });
          } else {
            wb = XLSX.read(binary, {
              type: 'binary'
            });
          }
          // outdata就是你想要的东西 excel导入的数据
          outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]);
          // excel 数据再处理
          let arr = []
          outdata.map(v => {
            let obj = {}
            obj.staff = v.员工名称
            obj.department = v.部门名称
            obj.morning = v.早上打卡时间
            obj.afternoon = v.下午打卡时间
            obj.record = v.记录时间
            arr.push(obj)
            _this.tableData.push(obj)
          })
        }
        reader.readAsArrayBuffer(f);
      }
      if (rABS) {
        reader.readAsArrayBuffer(f);
      } else {
        reader.readAsBinaryString(f);
      }
    }
  },created() {
    // 分页查询
    this.about();
    // 查询所有部门id 与 ，名称
    this.selectAllDept();
  },
};
</script>

<style>

table * {
  text-align: center;
}


.y {
  margin-top: 20px;
}

.head {
  margin-top: 2%;
}
.head-export{
  margin-top: 20px;
  margin-left: 20px;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 30px;
}
.upload-demo {
  display: inline-block;
  margin-left: 10px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff2?t=1640334127364') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff?t=1640334127364') format('woff'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.ttf?t=1640334127364') format('truetype');
}
</style>

