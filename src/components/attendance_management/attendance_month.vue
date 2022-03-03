<!--考勤月报表-->
<template>
  <!-- 月份报表页面 -->
  <div class="head">
    <div class="head-surface">
      <span><span style="color:#000000">{{getdate()}}</span>人事报表</span>
    </div>
    <div class="head-to">
      <el-tabs v-model="activeName">
        <el-tab-pane label="考勤统计" name="first"></el-tab-pane>
      </el-tabs>
    </div>
    <!--    搜索框-->
    <div class="head-examine">
      <el-input size="small" v-model="input" placeholder="请输入员/部名称" style="width:150px;">
      </el-input>

      <!--查询按钮-->
      <el-button @click="dimsearch()" style="background-color: #ffffff;border-radius: 30%; margin-left: 10px" size="small">
        <el-icon><i-search />
        </el-icon>
      </el-button>
      <!--导出导入-->
      <el-button @click="derive()" type="warning" plain size="small" style="margin-left: 820px">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>
      <el-button type="info" plain size="small">
        归档{{getdate()}}份报表
      </el-button>
    </div>
<!--表格-->
    <div class="bottom">
      <el-table :data="tableData" stripe style="width: 100%" border
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">
        <el-table-column prop="staffName"  label="名称"/>
        <el-table-column prop="clssName"  label="部门"/>
        <el-table-column prop="normalFrequency" label="正常次数"/>
        <el-table-column prop="lateFrequency" label="迟到次数"/>
        <el-table-column prop="leaveEarlyFrequency" label="早退次数"/>
        <el-table-column prop="absenteeismFrequency" label="旷工次数"/>
        <el-table-column prop="present" label="是否全勤"/>
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
          @size-change="Arch"
          @current-change="Arch"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import {ElMessage} from "element-plus/es";
import {ElMessageBox} from "element-plus";
import {export_json_to_excel} from '/src/excal/Export2Excel.js'
export default {
  data() {
    return {
      input:"",
      tableData:[],
      activeName: "first",
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      updateTime:""
    };
  },
  methods:{
    //分页查询
    Arch(){
      this.axios
          .get("http://localhost:8010/provider/archive/ss/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize)
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
    //模糊搜索
    dimsearch(){
      // 判断文本框为空
      if ( this.input == "" || this.input == null){
        this.Arch()
        return
      }
      this.axios
          .get("http://localhost:8010/provider/archive/dim/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize+"/"+this.input)
          .then((response) =>{
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
          }).catch(function (error){
        console.log(error)
      })

    },
    //获取当前年月日
    getdate() {
      var date = new Date();
      var seperator1 = "-";
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var strDate = date.getDate();

      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      var currentdate = year + " 年 " + month + " 月 ";
      return currentdate;
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
        ElMessage.success("取消成功")
      })
    },
    // 导出方法
    deriveExcel() {
      var _this = this;
      let tHeader = ["名称", "部门", "正常次数", "迟到次数", "早退次数","旷工次数","是否全勤"]; // 导出的表头名
      let filterVal = ["staffName", "clssName", "normalFrequency", "lateFrequency", "absenteeismFrequency","leaveEarlyFrequency","present"];//导出其prop属性
      ElMessageBox.prompt('请输入文件名', '提示', {
        confirmButtonText: '生成',
        cancelButtonText: '取消',
      }).then(({value}) => {
        let data = _this.formatJson(filterVal, _this.tableData);
        export_json_to_excel(tHeader, data, value);
        ElMessage.success("生成成功")
      })
          .catch(() => {
            ElMessage.success("失败成功")
          })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map((v) => filterVal.map((j) => v[j]));
    },
  },
  //
  created() {
    this.Arch();
  },
};
</script>

<style scoped>
.head {
  margin-top: 7px;
  margin-left: 20px;
  margin-right: 5px;
  margin-bottom: 10px;
}


.head-surface {
  font-size: 20px;
  text-align: center;
  margin-top: 20px;
}

.head-to {
  margin-left: 10px;
}

.head-examine {
  margin-left: 30px;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

.bottom {
  margin-left: 18px;
  margin-top: 20px;
  margin-right: 5px;
  margin-bottom: 10px;
  padding: 1%;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 5px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff2?t=1640334127364') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff?t=1640334127364') format('woff'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.ttf?t=1640334127364') format('truetype');
}
</style>