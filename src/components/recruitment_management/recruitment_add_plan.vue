<!-- 新增招聘计划 -->

<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <!--标题-->
      <div class="j-card-head">
        <div class="j-card-head-title">
          <span>{{ this.$route.query.name }}招聘计划</span>
        </div>
      </div>
      <!--内容-->
      <div class="j-card-body ">
        <span></span>
        <div class="sub-Content__primary ">
          <div style="width: 50%; margin: auto;">
            <el-form ref="ruleForm" :model="ruleForm" :rules="rules" label-width="120px"
                     class="demo-ruleForm">


              <el-form-item label="招聘计划名称" prop="zpname" style="width:500px;">
                <el-input v-model="ruleForm.zpname"></el-input>
              </el-form-item>

              <el-form-item label="需求部门" prop="zpdeptName">
                <div style="display: inline-block;margin-left:25px">

                  <el-select v-model="ruleForm.zpdeptName" ref="vueSelecto" @click="onclickso()" placeholder="请选择部门"
                             style="width: 240px;">
                    <el-option hidden></el-option>
                    <el-tree :data="deptlists"
                             show-checkbox
                             :default-expand-all=true
                             :check-on-click-node=true
                             :check-strictly=true
                             node-key="deptId"
                             :props="defaultProps" ref="treeo" @check-change="handleCheckChangeo()"/>

                  </el-select>

                </div>
              </el-form-item>
              <!--              //需求职位-->
              <el-form-item label="需求职位:" prop="position1">
                <el-select v-model="ruleForm.position1" placeholder="请选择职位" style="width: 240px;">
                  <el-option
                      v-for="item in positionAll"
                      :key="item.positionId"
                      :label="item.positionName"
                      :value="item.positionId"
                  >
                  </el-option>

                </el-select>
              </el-form-item>


              <el-form-item label="需招聘人数" prop="zpnum">
                <el-input-number
                    v-model="ruleForm.zpnum"
                    :min="1"
                    :max="50"
                    controls-position="right"
                />
              </el-form-item>

              <el-form-item label="最低学历" prop="zpxl">
                <el-select v-model="ruleForm.zpxl" placeholder="请选择">
                  <el-option label="不限" value="不限"></el-option>
                  <el-option label="初中" value="初中"></el-option>
                  <el-option label="高中" value="高中"></el-option>
                  <el-option label="大专" value="大专"></el-option>
                  <el-option label="本科" value="本科"></el-option>
                  <el-option label="硕士" value="硕士"></el-option>
                  <el-option label="博士" value="博士"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="计划开始时间" required style="width:380px">
                <el-date-picker
                    v-model="ruleForm.timef"
                    type="daterange"
                    start-placeholder="Start Date"
                    end-placeholder="End Date">
                </el-date-picker>
              </el-form-item>

              <el-form-item label="月薪范围" prop="zpxqf">
                <el-select v-model="ruleForm.zpxqf" placeholder="Activity zone">
                  <el-option label="3000-5000" value="3000-5000"></el-option>
                  <el-option label="5000-8000" value="5000-8000"></el-option>
                  <el-option label="8000-12000" value="8000-12000"></el-option>
                  <el-option label="12000以上" value="12000以上"></el-option>
                </el-select>
              </el-form-item>


              <el-form-item>
                <el-button style="width: 100px;" type="primary" @click="submitForm('ruleForm')">提交</el-button>
                <el-button style="margin-left: 30px; width: 100px" @click="goback()">取消</el-button>
              </el-form-item>

            </el-form>
          </div>


        </div>
      </div>

    </div>
  </div>
</template>

<script lang="ts">
import {ref} from 'vue'
import {ElMessage} from "element-plus/es";

export default {
  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value: 'deptId'
    }
    const zpnum = ref(1)
    return {

      reso: "",
// 选中值1
      res1o: "",
// 选中值2
      res2o: "",
// 部门  文本框的值
      depto: "",
// 格式
      defaultProps,

      positionAll: [],
      //存招聘计划信息
      recruitmentPlanMsg: [],
//存放部门信息
      deptlists: [],
      ruleForm: {
        //计划名称
        zpname: '',
        //需求部门id
        zpdept: "",
        //需求部门name
        zpdeptName: "",
        //职位
        zpzw: '',
        //人数
        zpnum: 1,
        //学历
        zpxl: '',
        //时间范围
        timef: '',
        //薪酬范围
        zpxqf: '',
        //部门职位
        position1: [],
      },
      rules: {
        zpname: [{
          required: true,
          message: '招聘计划名称不能为空!!!',
          trigger: 'blur',
        },
        ],
        zpdeptName: [{
          required: true,
          message: '请选择需求部门!!!',
          trigger: 'change',
        },],
        zpzw: [{
          required: true,
          message: '请设置招聘职位!!!',
          trigger: 'change',
        },],
        zpnum: [{
          required: true,
          message: '计划招聘人数不能为空!!!',
          trigger: 'blur',
        },],
        zpxl: [{
          required: true,
          message: '最低学历不能为空!!!',
          trigger: 'change',
        },],
        timef: [{
          required: true,
          message: '请选择招聘时间范围!!!',
          trigger: 'change',
        },],
        zpxqf: [{
          required: true,
          message: '请选择薪酬范围!!!',
          trigger: 'change',
        },],
        // position1: [{
        //   required: true,
        //   message: '请选择职位!!!',
        //   trigger: 'blur',
        // }, ],
      },
    }
  },
  methods: {
    //新增考勤扣款方案
    // insertZp(){
    //   alert(this.attendandceForm.attendandceName)
    //   this.axios({
    //     url: 'http://localhost:8010/provider/recruitmentPlanVo/insertzpjh',
    //     method: 'post',
    //     data:{
    //
    //       // 招聘计划名称
    //       recruitmentPlanName:this.ruleForm.zpdeptName,
    //       //部门编号
    //       deptId:this.ruleForm.zpdept,
    //       //最低学历
    //       educationName:this.ruleForm.zpxl,
    //       //需招聘人数
    //       recruitmentPlanNumber:this.ruleForm.zpnum,
    //       //开始时间
    //       recruitmentPlanStartTime:this.ruleForm.timef,
    //       //结束时间
    //       recruitmentPlanEndTime:this.num5,
    //       //月薪范围
    //       recruitmentPlanSalary:this.attendandceForm.attendandceRemark,
    //       //招聘计划状态
    //       recruitmentZt:0,
    //       //负责人编号
    //       staffId:1,
    //     }
    //   }).then(response => {
    //     if (response.data.data > 0) {
    //       ElMessage({
    //         message: '添加成功',
    //         type: 'success',
    //       })
    //       this.$router.go('-1');
    //     } else {
    //       ElMessage.error('添加失败')
    //     }
    //   }).catch(function (error) {
    //     console.log(error);
    //   });
    // },

    reVal() {

      let reVal = {

        // 招聘计划名称
        recruitmentPlanName: this.ruleForm.zpname,
        //部门编号
        deptId: this.ruleForm.zpdept,
        // 职位
        postId: this.ruleForm.position1,
        //最低学历
        educationName: this.ruleForm.zpxl,
        //需招聘人数
        recruitmentPlanNumber: this.ruleForm.zpnum,
        //开始时间
        recruitmentPlanStartTime: this.ruleForm.timef[0],
        //结束时间
        recruitmentPlanEndTime: this.ruleForm.timef[1],
        //月薪范围
        recruitmentPlanSalary: this.ruleForm.zpxqf,
        //招聘计划状态
        recruitmentZt: 0,
        //负责人编号
        staffId: 1

      }

      this.add(reVal)

    },
    //根据id查询招聘计划信息
    selectupdateRecruitmentPlanId() {
      this.axios
          .get("http://localhost:8010/provider/recruitmentPlan/selectRectruitmentPlanId/" + this.$route.query.id)
          .then((response) => {
            console.log("1111111111")
            console.log(response);
            this.recruitmentPlanMsg = response.data.data;

            // 招聘计划名称
            this.ruleForm.zpname = this.recruitmentPlanMsg.recruitmentPlanName
            //部门编号
            this.$refs.treeo.setCheckedKeys([this.recruitmentPlanMsg.deptId], false)

            // 职位
            this.ruleForm.position1 = this.recruitmentPlanMsg.positionId
            //最低学历
            this.ruleForm.zpxl = this.recruitmentPlanMsg.educationName
            //需招聘人数
            this.ruleForm.zpnum = this.recruitmentPlanMsg.recruitmentPlanNumber

            //开始时间          //结束时间
            this.ruleForm.timef=[this.recruitmentPlanMsg.recruitmentPlanStartTime,this.recruitmentPlanMsg.recruitmentPlanEndTime]

            //月薪范围
            this.ruleForm.zpxqf = this.recruitmentPlanMsg.recruitmentPlanSalary
          })
          .catch(function (error) {
            console.log(error);
          });
    },
   // 修改招聘计划
    updateRecruitmentPlan(){
      this.axios({
        url: 'http://localhost:8010/provider/recruitmentPlan/updateRectruitmentPlan',
        method: 'put',
        data: {
          //招聘计划id
          recruitmentPlanId:this.$route.query.id,
          //部门编号
          deptId:this.ruleForm.zpdept,
          // 职位
          positionId:this.ruleForm.position1,
          //最低学历
          educationName:this.ruleForm.zpxl,
          //需招聘人数
          recruitmentPlanNumber:this.ruleForm.zpnum,
          //开始时间
          recruitmentPlanStartTime:this.ruleForm.timef[0],
          //结束时间
          recruitmentPlanEndTime:this.ruleForm.timef[1],
          //月薪范围
          recruitmentPlanSalary:this.ruleForm.zpxqf,


        }
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          this.$router.go('-1');
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
//查询职位名称
    selectPositionName() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectPositionName")
          .then((response) => {
            console.log(response);
            this.positionAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    //添加
    add(reVal) {
      this.axios
          .post("http://localhost:8010/provider/recruitmentPlan/insertzpjh", reVal)
          .then((response) => {

            if (response.data.data > 0) {
              ElMessage.success("添加成功")
              this.goback()
            } else {
              ElMessage.error("添加失败")
            }
          }).catch(function (error) {
        console.log(error);
      })
    },

    onclickso() {
      // 点击文本框时调用的方法
      // 取当前选择器中的复选框选项id
      this.res1o = this.$refs.treeo.getCheckedKeys()
    },

//节点选中状态发生变化时的回调
    handleCheckChangeo(data, checked, indeterminate) {

      // 节点选中状态发生变化
      // 取当前选择器中的复选框选项id
      this.res2o = this.$refs.treeo.getCheckedKeys()

      // 取差集
      let a = new Set(this.res1o);
      let b = new Set(this.res2o);
      let arr = Array.from(new Set([...b].filter(x => !a.has(x))));

      // 将差集赋值上选择器
      this.$refs.treeo.setCheckedKeys([arr], false)

      //获取所有选中的节点 start
      this.reso = this.$refs.treeo.getCheckedNodes()
      this.reso.forEach((item) => {
        // 赋值到文本框
        this.ruleForm.zpdeptName = item.deptName
        this.ruleForm.zpdept = item.deptId
        // 关闭选择器
        this.$refs.vueSelecto.blur();
      })
    },
    //查询部门名称
    selectDeptName() {
      this.axios
          .get("http://localhost:8010/provider/dept/selectAll")
          .then((response) => {
            console.log(response);
            this.deptlists = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    //提交按钮
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if(this.$route.query.name='修改'){
            this.updateRecruitmentPlan()
          }else{

            this.reVal();
          }

        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    //取消：跳转上一级目录
    goback() {
      this.$router.go('-1');
    }
  },
  created() {
    this.selectDeptName()
    this.selectPositionName()
    if (this.$route.query.name == '修改') {
      this.selectupdateRecruitmentPlanId()
    }
  }
}
</script>


<style type="text/css" scoped>
.j-card-head {
  height: 48px;
  line-height: 48px;
  background: #fff;
  border-bottom: 1px solid #e9e9e9;
  margin: 0 24px;
  overflow: hidden;
}

/deep/ .el-form-item {
  display: flex;
  margin-bottom: 35px;
}

.j-card-head-title {
  float: left;
  font-size: 18px;
  display: inline-block;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  color: rgba(0, 0, 0, 0.85);
  font-weight: 500;
  letter-spacing: 0.3px;
}

.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

.saas-main-content {
  padding-top: 12px;
  min-height: 500px;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
}

.xxx {
  display: none;
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
</style>
