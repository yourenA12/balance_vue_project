<!-- 招聘计划 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body ">
        <span></span>

        <!-- 表格按钮部分 -->
        <div class="mt-20 ml-20 mr-20">
          <!-- 新增招聘计划按钮 -->

          <a style="margin-top: 50px;">
            <router-link :to="{path:this.one,query:{path:this.$route.query.path,name:'新增'}}">

              <el-button size="small" type="primary"  plain style="position: absolute;">
                <el-icon><i-plus/></el-icon>
                新增
              </el-button>

            </router-link>
          </a>

          <!-- 批量导入按钮 -->

          <el-button  size="small" type="warning" plain style="position: absolute;left: 100px;">
            <el-icon><i-download /></el-icon>
            批量导入
          </el-button>
<!--          <button style="position: absolute;left: 100px; color: #ffba00;-->
<!--                  background: #fff8e6;-->
<!--                  border-color: #ffe399;-->

<!--               "-->
<!--                  type="button" class="ant-btn abt"-->
<!--                 >-->
<!--            <el-icon><i-download /></el-icon>-->
<!--            <span></span>-->
<!--          </button>-->

          <!-- 下拉选择器 -->
          <div class="resume-operation">

            <!--搜索输入框-->
            &nbsp;&nbsp;&nbsp;
            <el-input style="width: 200px;border-radius: 30%;" v-model="pageInfo.recruitmentPlanName" placeholder="状态 \ 部门名称 \ 招聘计划" clearable/>
            <!--搜索按钮-->
            &nbsp;&nbsp;&nbsp;
            <el-button @click="selectrecruitment_plan()" style="background-color: #ffffff;border-radius: 30%;" size="small">

              <el-icon><i-search />

              </el-icon>
            </el-button>
          </div>
          </div>

        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="tableData" stripe style="width: 100%;" :header-cell-style="{background:'#eef1f6',color:'#606266'}" >
<!--          <el-table :data="tableData" style="width: 100%; cursor: pointer" size="mini" :header-cell-style="{background:'#eef1f6',color:'#606266'}">-->
            <el-table-column prop="recruitmentPlanId" label="序号" width="150"/>
            <el-table-column  prop="recruitmentPlanName" label="招聘计划名称" width="200">
              <template #default="scope">
                <router-link :to="{path:this.two,query:{path:this.$route.query.path,recruitmentPlanName:scope.row.recruitmentPlanName}}">{{scope.row.recruitmentPlanName}}</router-link>
              </template>
            </el-table-column>
            <el-table-column prop="positionName" label="招聘职位" width="200"/>
            <el-table-column prop="deptName" label="需求部门" width="200"/>
            <el-table-column prop="recruitmentPlanNumber" label="招聘人数" width="200"/>
            <el-table-column prop="recruitmentPlanStartTime" label="发布时间" width="200"/>
            <el-table-column label="招聘状态" width="200">
              <template #default="scope">
                {{scope.row.recruitmentZt=='0'?'招聘中':'已结束'}}
              </template>
            </el-table-column>
            <el-table-column fixed="right" label="操作" width="180">
              <template #default="scope">
                <div v-if="tableData[scope.$index].recruitmentZt==0">

                  <el-button type="text" size="small" @click="updateRecruitment(scope.row.recruitmentPlanId)" >编辑</el-button>

                  &nbsp;
                  <el-popconfirm title="是否确定关闭?" @confirm="confirmgb(scope.row)" @cancel="cancelsc()">
                    <template #reference>
                      <el-button type="text" size="small" >关闭</el-button>

                    </template>
                  </el-popconfirm>

                </div>
              </template>
            </el-table-column>
          </el-table>

        </div>
      </div>
      <div class="demo-pagination-block">
        <!-- <span class="demonstration">All combined</span> -->
        <el-pagination
            v-model:currentPage="pageInfo.currenPage"
            :page-sizes="[3, 5, 10, 50]"
            v-model:page-size="pageInfo.pagesize"
            :default-page-size="pageInfo.pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="pageInfo.total"
            :pager-count="5"
            background
            @size-change="selectrecruitment_plan"
            @current-change="selectrecruitment_plan"

        >
        </el-pagination>
      </div>
    </div>


</template>

<script>
import { ElMessage } from 'element-plus'
export default {
  data() {
    return {
      one: '/recruitment/addplan/addplan',
      two:'/recruitment/plan/details',
      //分页
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 3,
        recruitmentPlanName:"",
        recruitmentPlanId:""
      },
      w:{
        recruitmentPlanId:"",
      },

      //下拉选择器
      options1: [
        {value: '人力资源师', label: '人力资源师'},
        {value: '数据挖掘工程师', label: '数据挖掘工程师'},
        {value: '法务经理', label: '法务经理'},
        {value: '测试工程师', label: '测试工程师'}
      ],
      options2: [
        {value: '招聘中', label: '招聘中'},
        {value: '已结束', label: '已结束'}
      ],
      value1: "",
      value2: "",
      row1:{
        ID: null,
        zpname: null,
        zpzw: null,
        zpdept: null,
        zpnum: null,
        statetime: null,
        zpzt: null
      },
      //输入框数据
      input: "",
      tableData: [],
    }
  },
  methods:{
    //消息提示框确认按钮事件
    confirmgb(row){
      this.deletejh(row)
    },
    //消息提示框取消按钮事件
    cancelgb(){
      ElMessage({
        message: '已取消该操作',
        type: 'warning',
      })
    },
    //消息提示框确认按钮事件
    confirmsc(row){

      /*ElMessage({
        message: '操作成功',
        type: 'success',
      })*/
    },
    //消息提示框取消按钮事件
    cancelsc(){
      ElMessage({
        message: '已取消该操作',
        type: 'warning',
      })
    },
    selectrecruitment_plan(){
      this.axios
      .get("http://localhost:8010/provider/recruitmentPlanVo/queryPage",{params:this.pageInfo})
      .then((response) => {
        console.log(response);
        this.tableData = response.data.data.records;
        console.log(response.data.data.records);
        this.pageInfo.total = response.data.data.total;
      })
      .catch(function (error){
        console.log(error);
      })
    },
    //删除
    deletejh(row){
        this.axios
            .delete("http://localhost:8010/provider/recruitmentPlan/jihua/" + row.recruitmentPlanId)
            .then((response) => {
              console.log(response)
              if (response.data.data === "成功") {
                ElMessage.success("删除成功")
                this.selectrecruitment_plan()
              } else {
                ElMessage.error("删除失败")
              }
            })


    },
    //修改招聘计划
    updateRecruitment(row){
      //跳转页面路径
      this.$router.push({path:this.one,query:{path:this.$route.query.path,name: '修改',id:row}})
  }

  },
  created() {
    this.selectrecruitment_plan();
  }
}

</script>


<style type="text/css" scoped>
@import url("../../css/zpdaohang.css");
.demo-pagination-block{
  margin-left:850px ;
  margin-top: 20px;
  margin-bottom: 30px;
}
.saas-main-content {
  padding-top: 12px;
  min-height: 500px;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.j-card:hover {
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  border-color: transparent;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
  border-top-color: rgb(233, 233, 233);
  border-right-color: rgb(233, 233, 233);
  border-bottom-color: rgb(233, 233, 233);
  border-left-color: rgb(233, 233, 233);
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.mr-20 {
  margin-right: 20px;
  position: relative;
}

.ml-20 {
  margin-left: 20px;
}

.mt-20 {
  margin-top: 20px;
}

a {
  color: #6ba1e0;
  background-color: transparent;
}

a, area, button, [role="button"], input:not([type="range"]), label, select, summary, textarea {
  touch-action: manipulation;
}

a {
  color: #72b3c4;
  text-decoration: none;
  background-color: transparent;
  outline: none;
  cursor: pointer;
  transition: color 0.3s;
  -webkit-text-decoration-skip: objects;
}

button::-moz-focus-inner, [type="button"]::-moz-focus-inner, [type="reset"]::-moz-focus-inner, [type="submit"]::-moz-focus-inner {
  border-style: none;
}

button::-moz-focus-inner, [type="button"]::-moz-focus-inner, [type="reset"]::-moz-focus-inner, [type="submit"]::-moz-focus-inner {
  padding: 0;
  border-style: none;
}

.ant-btn::before {
  background: #fff;
  border-radius: inherit;
}

.ant-btn::before {
  position: absolute;
  top: -1px;
  right: -1px;
  bottom: -1px;
  left: -1px;
  z-index: 1;
  display: none;
  background: #fff;
  border-radius: inherit;
  opacity: 0.35;
  transition: opacity 0.2s;
  content: '';
  pointer-events: none;
}

button, html [type="button"], [type="reset"], [type="submit"] {
  -webkit-appearance: button;
}

.ant-btn-primary {
  color: #fff;
  background-color: #085fc3;
  border-color: #085fc3;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}

.ant-btn-primary {
  color: #fff;
  background-color: #366cb3;
  border-color: #366cb3;
  text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.12);
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}

.ant-btn, .ant-btn:active, .ant-btn:focus {
  outline: 0;
}

.ant-btn {
  line-height: 1.499;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  border: 1px solid transparent;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  user-select: none;
  touch-action: manipulation;
  height: 32px;
  padding: 0 15px;
  font-size: 14px;
  border-radius: 3px;
  color: #606c82;
  background-color: #fff;
  border-color: #d3dae2;
}

.ant-btn-primary {
  color: #fff;
  background-color: #085fc3;
  border-color: #085fc3;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
}


/**
 * 下拉选择器样式
 */
.resume-operation {
  text-align: right;
  padding-left: 8px;
}

.resume-operation .ant-form-item {
  display: inline-block;
  margin-right: 10px;
  margin-bottom: 4px;
  width: 200px;
}

.ant-form-item {
  color: rgba(0, 0, 0, 0.65);
}


.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

.abt:hover {
  border: 1px solid #085FC3;
  color: #085FC3;
}

/deep/ .el-table th.el-table__cell > .cell {
  display: inline-block;
  box-sizing: border-box;
  position: relative;
  vertical-align: middle;
  width: 100%;
  font-weight: initial;
  color: black;
}


/**
修改样式
 */

</style>

