<!--待入职员工页面-->
<template>
  <div class="saas-main-content">

    <!--搜索输入框-->
    <el-row style="width:200px;float:right;">
      <el-input v-model="input3" placeholder="搜索">
        <template #suffix>
          <el-icon class="el-input__icon">
            <i-search/>
          </el-icon>
        </template>
      </el-input>
    </el-row>

    <div style="width:100px;height:30px;margin-top:40px;"></div>
    <el-table :data="tableData" style="width: 100%"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}">
      <el-table-column prop="resumeName" label="姓名" width="180"/>
      <el-table-column prop="deptName" label="部门" width="180"/>
      <el-table-column prop="postName" label="职位" width="180"/>
      <el-table-column prop="resumePhone" label="手机号" width="180"/>
      <el-table-column prop="resumeMailbox" label="邮箱" width="180"/>
      <el-table-column prop="hiredate" label="入职日期" width="180"/>
      <el-table-column label="操作">
        <template #default="scope">

          <!-- 删除行确认框 -->
          <el-popconfirm
              confirm-button-text="Yes"
              cancel-button-text="No"
              title="确定要入职吗？"
              @confirm="insert_val(scope.row)"
          >
            <template #reference>
              <el-button type="text" size="small">入职</el-button>
            </template>
          </el-popconfirm>

          <el-button @click="ResumeQZ(scope.row)" type="text" size="small">放弃</el-button>

        </template>
      </el-table-column>
    </el-table>

    <!-- 分页插件 -->
    <div class="demo-pagination-block">
      <el-pagination
          v-model:currentPage="pageInfo.currentPage"
          :page-sizes="[3, 5, 10, 50]"
          v-model:page-size="pageInfo.pagesize"
          :default-page-size="pageInfo.pagesize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="pageInfo.total"
          :pager-count="5"
          background
          @size-change="selectEmps"
          @current-change="selectEmps"
      >
      </el-pagination>
    </div>

    <!--  弹框  -->
    <div>
      <el-dialog
          v-model="become"
          width="30%"
          :close-on-click-modal="false">
        放弃原因：
        <el-input type="textarea" v-model="give_reason" style="width:240px;"></el-input>
        <div style="margin-top:30px;margin-left: 120px;">
          <el-button @click="become=false">取消</el-button>
          <el-button @click="updateResum()" type="primary">确定</el-button>
        </div>

      </el-dialog>
    </div>


  </div>
</template>

<script>
 import {defineComponent, ref} from 'vue'
 import {ElMessage} from "element-plus";

export default {
  data() {
    return {
      become: false,
      input3: "",
      give_reason:"",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      tableData:[],

      // form: [
      //   {
      //     desc: ""
      //   }
      // ]

      staffVal:null,
      workVal:null,
      educationVal:null,
      resumeVal:null,
      employmentTableVue:null,

    }
  },
  methods: {
    //多表查询
    selectEmps() {
      this.axios
          .get("http://localhost:8010/provider/entryhirdeVo/selectEntryhirdeVo/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //入职前调用，取值方法
    insert_val(row){

        // 员工表数据
        this.staffVal={
          //姓名
          staffName:row.resumeName,
          //性别
          staffSex:row.resumeSex,
          //电话号码
          staffPhone:row.resumePhone,
          //邮箱
          staffEmail:row.resumeMailbox,
          //学历
          staffEducation:row.resumeEducation,
          //照片
          staffPicture:row.resumePhoto,
          //出生日期
          staffBirthday:row.resumeBirthday,
          //户口所在地
          staffRegistered:row.resumeResidence,
          //政治面貌
          staffOutlook:row.resumePoliticalOutlook,
          //入职日期
          staffHiredate:row.hiredate,
          //部门
          deptId:row.deptId,
          //职位
          deptPostId:row.deptPostId,
          //转正日期
          // workerDate:row.hiredate+,
          //工龄

        }

        // 工作经历表数据
      this.workVal={
        //开始时间
        workStareTime:row.workStareTime,
        //结束时间
        workEndTime:row.workEndTime,
        //员工外键
        // STAFF_ID:row.employmentId,
        //公司名称
        companyName:row.companyName,
        //职位名称
        positionName:row.positionName,
        //所属行业
        positionIndustry:row.positionIndustry,
        //工作描述
        positionDescribe:row.positionDescribe,
        //税前月薪
        positionSqmonthly:row.positionSqmonthly,
      }

      // 教育经历表数据
      this.educationVal={
        //开始时间
        educationStartTime:row.educationStartTime,
        //结束时间
        educationEndTime:row.educationEndTime,
        //员工外键
        // STAFF_ID:row.employmentId,
        //学校名称
        educationStudentname:row.educationStudentname,
        //所属专业
        educationMajor:row.educationMajor,
        //是否全日制
        educationFullTime:row.educationFullTime,
      }

      // 修改简历表状态为已入职
      this.resumeVal={
        resumeId: row.resumeId,
        resumeZt: "9"
      }

      this.insertStaff(); // 调用添加方法

    },
    //添加员工
    insertStaff(){
      this.axios({
        url: 'http://localhost:8010/provider/staff/insertStaff',
        method: 'post',
        data:{
          Staff:this.staffVal,
          Work:this.workVal,
          Education:this.educationVal,
          Resume:this.resumeVal
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectEmps() // 修改完成后调用查询方法
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

  },
    //修改取简历表id、状态和录用原因
    ResumeQZ(row){
      //弹出放弃原因
      this.become=true

      // 修改简历表状态为已入职
      this.resumeVal={
        resumeId: row.resumeId,
        resumeZt: "10"
      }

      //修改录用表录用原因
      this.employmentTableVue={
        employmentId:row.employmentId,
        waiveReason:this.give_reason
      }

    },
    //修改简历状态和录用原因
    updateResum(){
      // 调用修改方法之前，再次取“放弃原因”文本框中的值
      this.employmentTableVue.waiveReason=this.give_reason
      if (this.employmentTableVue.waiveReason == "" || this.employmentTableVue.waiveReason == null){
        ElMessage.error('填写原因！！')
        return
      }

      this.axios({
        url: 'http://localhost:8010/provider/staff/updateResume',
        method: 'post',
        data:{
          Resume:this.resumeVal,
          EmploymentTable:this.employmentTableVue,

        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          this.selectEmps() // 修改完成后调用查询方法
          this.become=false // 关闭弹出
          this.give_reason="" //清空弹出框信息
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    }

  },
  created() {
    this.selectEmps();
  },
}
</script>

<style scoped>

/deep/ .mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

/deep/ .cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}


</style>
