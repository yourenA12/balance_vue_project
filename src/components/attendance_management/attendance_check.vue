<!--班次管理页面-->
<template>
  <div class="w">

    <!-- 搜索框 -->
    <div class="head">
    <b style="font-size: 18px">班次名称</b>
    <el-input v-model="input" placeholder="请输入班次名称：" style="width:200px; margin-left: 10px">
    </el-input>

      <!--查询按钮-->
      <el-button @click="dimsearch()" style="background-color: #ffffff;border-radius: 30%; margin-left: 20px" size="small">
        <el-icon><i-search />

        </el-icon>
      </el-button>


    </div>
    <!--新增按钮-->
    <div class="head-surface">
    <router-link :to="{path: this.one,query:{path:this.$route.query.path,way:'新增'}}">
      <el-button size="small" type="primary" plain>
        <el-icon><i-plus/></el-icon>
        新增
      </el-button>
    </router-link>
    </div>
    <!--  表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
        <el-table-column prop="classesName" label="班次名称"/>
        <el-table-column prop="classesBeginDate" label="上班时间" :formatter="dateFormat" />
        <el-table-column prop="classesEndDate" label="下班时间"  :formatter="dateFormat" />
        <el-table-column prop="classesState" label="状态">
          <template #default="scope">


            <el-button v-if="scope.row.classesState==0" size="mini" round type="primary" plain>启用</el-button>

            <el-button v-if="scope.row.classesState==1" size="mini" round type="danger" plain>禁用</el-button>


          </template>
        </el-table-column>
        <el-table-column prop="operate" label="操作">
          <template #default="scope">

            <el-button type="text" size="small" @click="redact(scope.row)">
              <el-icon><i-edit />
              </el-icon>修改
            </el-button>

            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定执行吗?"
                @confirm="through1(scope.row)"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon v-if="scope.row.classesState==0"><i-warning /></el-icon>
                  <el-icon v-if="scope.row.classesState==1"><i-switch-button /></el-icon>
                  {{ scope.row.classesState==1? '启用':'禁用' }}
                </el-button>
              </template>
            </el-popconfirm>

            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through2(scope.row)"
            >
              <template #reference>
                <el-button type="text" size="small" >
                  <el-icon><i-delete /></el-icon>删除
                </el-button>
              </template>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!--  分页-->
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
          @size-change="check"
          @current-change="check"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from 'vue'
import {ElMessage} from "element-plus";
export default {
  data() {
    return {
      input:"",
      // 新增路由地址
      one: '/attendance/check/classes/addclass',
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      tableData: [],

      value: ref(''),
    }
  },

  methods: {
    // 日期格式化
    dateFormat:function(row,column){
      let data = row[column.property]
      if (data == null) {
        return null
      }
      let dt = new Date(data)

      // 时分秒
      return dt.getHours() + ':' + dt.getMinutes() + ':' + dt.getSeconds()

      // 年月日 时分秒
      // return dt.getFullYear() + '-' + (dt.getMonth() + 1) + '-' + dt.getDate() + ' ' + dt.getHours() + ':' + dt.getMinutes() + ':' + dt.getSeconds()
    },
    // 点击编辑跳转
    redact(row) {
      this.$router.push({
        path: this.one,
        query: {
          path: this.$route.query.path,

          classesId:row.classesId,
          classesName: row.classesName,
          classesBeginDate: row.classesBeginDate,
          classesEndDate: row.classesEndDate,
          classesState:row.classesState,
          way:'修改'
        }
      })

    },
    //分页查询
    check(){
      this.axios
          .get("http://localhost:8010/provider/classes/fy/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize)
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

    //逻辑删除
    deletebc(row){
     // 如果 状态为启用 弹出保留一个班次状态
      if(row.classesState==0){
         ElMessage.error('请保留一个班次状态')
        return
      }
      this.axios
          .delete("http://localhost:8010/provider/classes/delete/" + row.classesId )
          .then((response)=>{
            console.log(response)
           if (response.data.data === "成功"){
             ElMessage.error("删除成功")
             this.check() // 删除完成后，查询一次
           }else {
              ElMessage.error("删除失败")
           }

          })
      .catch(function (error){
        console.log(error);
      })

    },

    //修改状态
    state(row){
      // 如果 状态为启用 弹出保留一个班次状态
      if(row.classesState==0){
        ElMessage.error('请保留一个班次状态')
        return
      }
      this.axios
          .put("http://localhost:8010/provider/classes/amend", {
            classesId:row.classesId,
            classesState: 0
          })
          .then((response) => {
            console.log(response)
            if( response.data.data ==="成功" ){
              ElMessage.success('修改成功')
              this.check() // 修改状态完成后，查询一次
            }else{
              ElMessage.error('修改失败')
            }
          }).catch(function (error){
        console.log(error);
      })

    },

    //模糊搜索
    dimsearch(){
      // 判断文本框为空
      if ( this.input == "" || this.input == null){
        this.check()
        return
      }
      this.axios
          .get("http://localhost:8010/provider/classes/dim/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize+"/"+this.input)
          .then((response) =>{
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
          }).catch(function (error){
            console.log(error)
      })

    },

    // 点击禁用确认按钮触发
    through1(row) {
      this.state(row)
    },
    // 点击删除确认按钮触发
    through2(row) {
      this.deletebc(row)
    },
  },
  created() {
    this.check();
  },

}
</script>

<style scoped>
/*居中*/
table * {
  text-align: center;
}

.head{
  margin-left: 10px;
  margin-top: 30px;
}

.head-surface{
  margin-left: 20px;
  margin-top: 20px;
}
.y{
  margin-top: 10px;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 30px;

}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_njgieohi6cg.woff2?t=1640333406121') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_njgieohi6cg.woff?t=1640333406121') format('woff'),
  url('//at.alicdn.com/t/font_3056280_njgieohi6cg.ttf?t=1640333406121') format('truetype');
}
</style>