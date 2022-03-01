<!--面试通过-->
<template>
  <div class="sub-Content__primary">
    <div class="ant-spin-nested-loading">
      <div class="ant-spin-container">
        <div class="mt-20 ml-20 mr-20">
          <!--筛选框-->

          <!--搜索框-->
          <div style="float: right;">
            <el-form :inline="true" :model="formInline" class="demo-form-inline">

              <el-form-item>
                <el-input v-model="formInline.user" placeholder="姓名、招聘计划名称" clearable></el-input>
              </el-form-item>

              <el-form-item>
                <el-button type="primary" @click="" size="mini"><i class="iconfont">&#xeafe;</i></el-button>
              </el-form-item>
            </el-form>
          </div>

        </div>
      </div>
      <!--
        筛选框
      -->

    </div>
    <br/>
    <!-- 表格数据 -->
    <div class="ant-table-wrapper j_statistics_layout">
      <el-table :data="tableData" stripe style="width: 100%;" :header-cell-style="{background:'#eef1f6',color:'#606266'}" >
        <el-table-column fixed="left"  align="center" type="selection" width="80" />
        <el-table-column fixed="left" prop="resumeName" label="姓名" width="150">
          <template #default="scope">
            <router-link :to="{path:this.details,query:{path:this.$route.query.path,resumeName:scope.row.resumeName}}">{{scope.row.resumeName}}</router-link>
          </template>
        </el-table-column>
        <el-table-column fixed="left" prop="positionName" label="投递部门" width="140"/>
        <el-table-column prop="resumeSex" label="性别" width="140"/>
        <el-table-column prop="resumeEducation" label="学历" width="140"/>
        <el-table-column prop="resumePhone" label="手机号" width="140"/>
        <el-table-column prop="resumeAge" label="年龄" width="140"/>
        <el-table-column prop="resumeMailbox" label="邮箱" width="140"/>
<!--        <el-table-column prop="professional" label="专业" width="140"/>-->
        <el-table-column prop="resumeBirthday" label="出生日期" width="140"/>
        <el-table-column prop="resumePoliticalOutlook" label="政治面貌" width="140"/>
        <el-table-column prop="resumeScoller" label="毕业学校" width="140"/>
<!--        <el-table-column prop="invitation" label="是否邀约" width="140"/>-->
        <el-table-column label="状态" width="140">
          <template #default="scope">
            <span v-if="scope.row.resumeZt==0">新简历</span>
            <span v-if="scope.row.resumeZt==1">候选人</span>
            <span v-if="scope.row.resumeZt==2">已邀约</span>
            <span v-if="scope.row.resumeZt==3">已淘汰</span>
            <span v-if="scope.row.resumeZt==4">面试中</span>
            <span v-if="scope.row.resumeZt==5">面试通过</span>
            <span v-if="scope.row.resumeZt==6">复试中</span>
            <span v-if="scope.row.resumeZt==7">待接受</span>
            <span v-if="scope.row.resumeZt==8">已接受</span>
            <span v-if="scope.row.resumeZt==9">已入职</span>
            <span v-if="scope.row.resumeZt==10">已拒绝</span>
            <span v-if="scope.row.resumeZt==11">放弃入职</span>
          </template>
        </el-table-column>


        <el-table-column fixed="right" label="操作" width="220">
          <template #default="scope">
            <div style="width: 200px">

              <el-button type="text"  size="small"  @click="insertEmpIds(scope.row)">
                录用
              </el-button>
              <el-drawer v-model="drawer">

                <!--入职时间-->
                <el-form
                    ref="ruleForm"
                    :model="ruleForm"
                    :rules="rules"
                    label-width="120px"
                    class="demo-ruleForm">
                  <el-form-item label="入职时间：" prop="Data" style="margin-left: -30px">
                    <el-date-picker v-model="ruleForm.Data" type="date" placeholder="Pick a day" >
                    </el-date-picker>
                  </el-form-item>

                <!--  试用期-->

                  <el-form-item label="试用期：" prop="syq" style="margin-left: -30px">
                    <el-input v-model="ruleForm.syq" disabled style="width: 300px"></el-input>

                  </el-form-item>

                  <!--试用期月薪 -->
                  <el-form-item label="试用期月薪：" prop="syyx" style="margin-left: -30px">
                    <el-input-number v-model="ruleForm.syyx" min="1" clearable style="width: 300px" ></el-input-number>

                  </el-form-item>

<!--                  转正月薪-->
                  <el-form-item label="转正月薪：" prop="zzyx" style="margin-left: -30px">
                    <el-input-number v-model="ruleForm.zzyx" min="1" clearable style="width: 300px" ></el-input-number>

                  </el-form-item>

<!--                  备注-->

                  <el-input
                      v-model="ruleForm.bz"
                      maxlength="200"
                      placeholder="填写评价"
                      show-word-limit
                      type="textarea"
                  />


                  <el-form-item>
                    <el-button style="width: 100px;"  type="primary" @click="submitForm('ruleForm')">提交</el-button>
                    <el-button  style="margin-left: 30px; width: 100px" @click="goback()">取消</el-button>
                  </el-form-item>
                  </el-form>

              </el-drawer>


              <el-button type="text" size="small" @click="Txiugai(scope.row)">淘汰/放弃</el-button>
            </div>

          </template>





        </el-table-column>
      </el-table>

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
            @size-change="selectinterviewPass_plan"
            @current-change="selectinterviewPass_plan"
        >
        </el-pagination>
      </div>
    </div>




  </div>

</template>

<script >
import {
  ref
} from 'vue'
import {ElMessage} from "element-plus";
// const drawer = ref(false)
const text = ref('')
const textarea = ref('')
export default {
  data() {
    return {
      details: '/recruitment/resume/details',
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      //修改状态
      fo:{
      resumeId:"",
      resumeZt:3
    },

      ruleForm: {
        resumeId:'',
        syq: '三个月',
        Data: '',
        syyx: '',
        zzyx: '',
        bz: ''
      },

      //简历
      resumeVul:null,
      employmentTableVul:null,


      drawer: false,
      //筛选框显示隐藏
      icons: false,
      //搜索框
      input: "",
      //表格数据
      tableData: [],
      //筛选框数据
      formInline: {
        vlues1: '',
        vlues2: '',
        vlues3: '',
        vlues4: '',
        user: ''

      },
      rules: {
        Data: [{
          required: true,
          message: '入职时间不能为空!!!',
          trigger: 'blur',
        },
        ],
        syyx: [{
          required: true,
          message: '试用工资不能为空!!!',
          trigger: 'blur',
        },
        ],
        zzyx: [{
          required: true,
          message: '转正月薪不能为空!!!',
          trigger: 'blur',
        },
        ],
      }
    }
  },
  methods:{


    selectinterviewPass_plan(){
      this.axios
          .get("http://localhost:8010/provider/ResumeVo/ResumePage_T/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            // console.log(response.data.data.records);
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error){
            console.log(error);
          })
    },
    insertEmpIds(row){
      this.drawer=true;
      this.ruleForm.resumeId=row.resumeId
    },

    //添加录用信息和修改简历状态
    insertEmployment_resume(){
      //添加录用表
      this.employmentTableVul={
        //简历编号
        resumeId:this.ruleForm.resumeId,

        //备注
        remarks:this.ruleForm.bz,

        //入职时间
        hiredate:this.ruleForm.Data,
        //使用期限
        probation:this.ruleForm.syq,

        //试用期月薪
        probationary:this.ruleForm.syyx,

        //转正月薪
        positiveMonthly:this.ruleForm.zzyx,

      }
      //修改简历表状态
      this.resumeVul={
        resumeId:this.ruleForm.resumeId,
        resumeZt: "8"
      }
      this.insertEmp()

    },


    //录用

    insertEmp(){

      this.axios({
        url: 'http://localhost:8010/provider/employmentTable/insertEmploymentTable',
        method: 'post',
        data:{
          Resume:this.resumeVul,
          EmploymentTable:this.employmentTableVul,
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.drawer=false
          this.selectinterviewPass_plan()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },

    //提交按钮
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.insertEmployment_resume()
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    //取消：跳转上一级目录
    goback(){
      this.$router.go('-1');
    },
    //淘汰
    Txiugai(row){

      this.fo.resumeId=row.resumeId

      this.axios
          .put("http://localhost:8010/provider/resume/resume/zeliminate",this.fo)
          .then((response) => {

            if( response.data.data ==="成功" ){
              ElMessage.success('修改成功')
              this.selectinterviewPass_plan()
            }else{
              ElMessage.error('修改失败')
            }
          }).catch(function (error){
        console.log(error);
      })
    },
  }, created() {
    this.selectinterviewPass_plan();
  }

}

</script>
<style type="text/css" scoped>
/*@import url("../../css/navigation.css");*/
@import url("../../css/zpdaohang.css");
.demo-pagination-block{
  margin-left:850px ;
  margin-top: 20px;
  margin-bottom: 30px;
}

.icon-s {
  width: 75%;
  height: 45px;
  border-radius: 4px;
  border: 1px solid #ddd;
  margin-top: 10px;
  padding: 16px;
  margin-left: 70px;
}
.el-button--primary { /* el-input 显示时 */
  background: #085fc3 !important;
  border-color: #085fc3 !important;
}

.el-button--primary:hover {/* el-input 悬浮时 */
  background: #297ccf !important;
  border-color: #297ccf !important;
  color: #FFF !important;
}

.j-tabs {
  border-bottom: 1px solid #d9d9d9;
}

.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

.ant-spin-nested-loading {
  position: relative;
}

.ant-spin-container {
  position: relative;
  transition: opacity 0.3s;
}

.ant-table-wrapper::after {
  clear: both;
}

.ant-table-wrapper::before, .ant-table-wrapper::after {
  display: table;
  content: '';
}

.abt:hover{
  color: #0c9c6e;
  border: 1px solid #0c9c6e;
}
</style>