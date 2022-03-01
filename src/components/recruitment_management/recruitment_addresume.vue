<!-- 新增简历-->

<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <!--标题-->
      <div class="j-card-head">
        <div class="j-card-head-title">

          <span >{{ this.$route.query.name }}简历</span>
        </div>
      </div>
      <!--内容-->
      <div class="j-card-body ">
        <div style="padding: 25px; height: 1000px">
          <div>
            <!-- 用户照片 -->
            <div style="width: 170px;">
              <div class="upload-btn common mb_10" v-if="!isShow">
                <label>
                  <input type="file" @change="uploadImg"/>
                </label>
              </div>
              <div class="img-list-item common mb_10" v-if="isShow">
                <img :src="src" class="common">
                <i class="del-img" @click="forkImage">
                </i>
              </div>
            </div>

            <!-- 用 户 基 本 消 息 -->
            <el-form :inline="true"  :rules="rules" ref="formInline"   :model="formInline" class="demo-form-inline"
                     style="width: 78%; margin-top: -225px; margin-left: 19%;">
              <h3>基本信息</h3>

              <br>
              <el-form-item label="姓 名：" prop="name">
                <el-input v-model="formInline.name"></el-input>
              </el-form-item>
              <el-form-item label="性 别：" prop="sex">
                <el-select v-model="formInline.sex" placeholder="请选择">
                  <el-option label="男" value="男"></el-option>
                  <el-option label="女" value="女"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="出生日期：" prop="day">
                <el-col :span="11">
                  <el-date-picker
                      v-model="formInline.day"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 200px"
                  ></el-date-picker>
                </el-col>
              </el-form-item>
              <el-form-item label="年 龄：" prop="adg">
                <el-input v-model="formInline.adg"></el-input>
              </el-form-item>
              <el-form-item label="毕业学校：" prop="byschool">
                <el-input v-model="formInline.byschool"></el-input>
              </el-form-item>

              <div style="display: block;">
                <el-form-item label="手机号：" prop="phone">
                  <el-input v-model="formInline.phone"></el-input>
                </el-form-item>
                <el-form-item label="邮 箱：" prop="mailbox">
                  <el-input v-model="formInline.mailbox"></el-input>
                </el-form-item>
                <el-form-item label="所在地：" prop="Location">
                  <el-input v-model="formInline.Location"></el-input>
                </el-form-item>
              </div>


              <div style="display: block;">


                <el-form-item label="学 历：" prop="education">
                  <el-select v-model="formInline.education" placeholder="请选择">
                    <el-option label="初中" value="初中"></el-option>
                    <el-option label="高中" value="高中"></el-option>
                    <el-option label="大专" value="大专"></el-option>
                    <el-option label="本科" value="本科"></el-option>
                    <el-option label="硕士" value="硕士"></el-option>
                    <el-option label="博士" value="博士"></el-option>
                  </el-select>
                </el-form-item>

                <el-form-item label="政治面貌：">
                  <el-select v-model="formInline.politics" placeholder="请选择">
                    <el-option label="群众" value="群众"></el-option>
                    <el-option label="共青团" value="共青团"></el-option>
                    <el-option label="党员" value="党员"></el-option>
                  </el-select>
                </el-form-item>

<!--                <el-form-item label="招聘计划：" prop="plan">-->
<!--                  <el-select v-model="formInline.zpplan" placeholder="请选择">-->
<!--                    <el-option label="Hr人力资源专员" value="Hr人力资源专员"></el-option>-->
<!--                  </el-select>-->
<!--                </el-form-item>-->
                <el-form-item label="招聘计划:" prop="recruitmentPlanId">
                  <el-select v-model="formInline.recruitmentPlanId" placeholder="请选择" style="width: 240px;">
                    <el-option
                        v-for="item in zpjhlists"
                        :key="item.recruitmentPlanId"
                        :label="item.recruitmentPlanName"
                        :value="item.recruitmentPlanId"
                    >
                    </el-option>

                  </el-select>
                </el-form-item>

              </div>
            </el-form>

          </div>
          <br>
          <div style="border: 1px solid #ebebeb; width: 100%; height: 0px ;"></div>
          <br>


          <div>
            <span>
              <h3>教育消息</h3>
              <br>
            </span>

            <el-form :inline="true" :rules="rules" ref="formInline1" :model="formInline" class="demo-form-inline">

              <el-form-item label="学校名称：" prop="school">
                <el-input v-model="formInline.school"></el-input>
              </el-form-item>

              <el-form-item label="所学专业：" prop="major">
                <el-input v-model="formInline.major"></el-input>
              </el-form-item>

              <el-form-item label="开始时间：" prop="start">
                <el-col :span="11">
                  <el-date-picker
                      v-model="formInline.start"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 200px"
                  ></el-date-picker>
                </el-col>
              </el-form-item>

              <el-form-item label="结束时间：" prop="end">
                <el-col :span="11">
                  <el-date-picker
                      v-model="formInline.end"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 200px"
                  ></el-date-picker>
                </el-col>
              </el-form-item>

              <el-form-item label="是否全日制：" prop="full_time">
                <el-radio-group v-model="formInline.full_time">
                  <el-radio :label="0">是</el-radio>
                  <el-radio :label="1">否</el-radio>
                </el-radio-group>
              </el-form-item>
            </el-form>
          </div>


          <br>
          <div style="border: 1px solid #ebebeb; width: 100%; height: 0px ;"></div>
          <br>

          <div>
            <span>
              <h3>工作经历</h3>
              <br>
            </span>


            <el-form :inline="true" :rules="rules" ref="formInline2" :model="formInline" class="demo-form-inline">
              <el-form-item label="公司名称：" prop="company">
                <el-input v-model="formInline.company"></el-input>
              </el-form-item>

              <el-form-item label="职位名称：" prop="position">
                <el-input v-model="formInline.position"></el-input>
              </el-form-item>


              <el-form-item label="开始时间：" prop="start1">
                <el-col :span="11">
                  <el-date-picker
                      v-model="formInline.start1"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 200px"
                  ></el-date-picker>
                </el-col>
              </el-form-item>

              <el-form-item label="结束时间：" prop="end1">
                <el-col :span="11">
                  <el-date-picker
                      v-model="formInline.end1"
                      type="date"
                      placeholder="请选择日期"
                      style="width: 200px"
                  ></el-date-picker>
                </el-col>
              </el-form-item>

              <el-form-item label="所属行业：" prop="industry">
                <el-input v-model="formInline.industry"></el-input>
              </el-form-item>

              <el-form-item label="税前月薪：" prop="salary">
                <el-input v-model="formInline.salary"></el-input>
              </el-form-item>

              <el-form-item label="工作描述：" style="display: block">
              <el-input
                  v-model="formInline.describe"
                  :rows="2"
                  type="textarea"
              />
              </el-form-item>
            </el-form>

            <el-button type="primary" @click="submitForm('formInline','formInline1','formInline2')">提交</el-button>
            <el-button  @click="resetForm('formInline','formInline1','formInline2')">重置</el-button>
            <el-button @click="goblack()">取消</el-button>

          </div>

        </div>
      </div>

    </div>
  </div>
</template>

<script>
import {ref} from 'vue'
import {ElMessage} from "element-plus";

export default {
  data() {
    return {
//储存简历
      resumesMsg:[],
      educationssMsg:[],
      workMsg:[],

      zpjhlists:[],
      src: '',
      isShow: false,
      formInline: {
        resumeId:'',
        educationssId:'',
        workExperiencessId:'',
        name:'',
        sex: '',
        day:'',
        adg:'',
        phone:'',
        mailbox:'',
        Location:'',
        education:'',
        byschool:'',
        politics:'',
        recruitmentPlanId:'',
        school:'',
        major:'',
        start:'',
        end:'',
        full_time:'',
        company:'',
        position:'',
        start1:'',
        end1:'',
        industry:'',
        salary:'',
        radio: ref(0),
        describe: ref(''),

      },

      rules: {
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
          { min: 2, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        sex: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ],
        day: [
          { type: 'date', required: true, message: '请选择出生日期', trigger: 'change' }
        ],
        phone: [
          {required: true, message: '请输入电话号码', trigger: 'change' },
          {min: 11, max: 11, message: '长度为十一个数字', trigger: 'blur'}
        ],
        mailbox: [
          { required: true, message: '请输入邮箱', trigger: 'change' }
        ],
        Location: [
          { required: true, message: '请输入户口所在地', trigger: 'change' }
        ],
        education: [
          { required: true, message: '请选择你的学历', trigger: 'change' }
        ],
        recruitmentPlanId:[
          {  required: true,
            message: '招聘计划名称不能为空!!!',
            trigger: 'change',}
        ],
        adg:[
          {
            required:true,
            message:'年龄不能为空',
            trigger:'change'
          }
        ],
        byschool: [
          {
            required:true,
            message:'学校不能为空',
            trigger:'change'
          }
        ],

        school:[
          {
            required:true,
            message:'学校名称不能为空！！',
            trigger:'change'
          }
        ],
        major:[
          {
            required:true,
            message:'专业不能为空！！',
            trigger:'change'
          }
        ],
        start:[
          {
            required:true,
            message:'开始时间不能为空！！',
            trigger:'change'
          }
        ],
        end:[
          {
            required:true,
            message:'结束时间不能为空！！',
            trigger:'change'
          }
        ],
        full_time:[
          {
            required:true,
            message:'是否全日制不能为空！！',
            trigger:'change'
          }
        ],

        company:[
          {
            required:true,
            message:'公司名称不能为空！！',
            trigger:'change'
          }
        ],
        position:[
          {
            required:true,
            message:'职位名称不能为空！！',
            trigger:'change'
          }
        ],
        start1:[
          {
            required:true,
            message:'开始时间不能为空！！',
            trigger:'change'
          }
        ],
        end1:[
          {
            required:true,
            message:'结束时间不能为空！！',
            trigger:'change'
          }
        ],
        industry:[
          {
            required:true,
            message:'所属行业不能为空！！',
            trigger:'change'
          }
        ],
        salary:[
          {
            required:true,
            message:'税前月薪不能为空！！',
            trigger:'change'
          }
        ]

      },

      resumeVal:null,
      workVal:null,
      educationVal:null,



    }
  },
  methods: {
/*    // 修改简历
    updateResumes(){
      alert(this.$route.query.id)
      this.axios({
        url: 'http://localhost:8010/provider/resume/updateResume',
        method: 'put',
        data: {
          //简历id
          resumeId: this.formInline.resumeId,
          //姓名
          resumeName: this.formInline.name,
          // 性别
          resumeSex: this.formInline.sex,
          //出生日期
          resumeBirthday: this.formInline.day,
          //年龄
          resumeAge: this.formInline.adg,
          //毕业学校
          resumeScoller:this.formInline.byschool,
          //手机号
         resumePhone: this.formInline.phone,
          //邮箱
         resumeMailbox: this.formInline.mailbox,
          //所在地
          resumeResidence: this.formInline.Location,
          //学历
          resumeEducation:this.formInline.education,
          //政治面貌
          resumePoliticalOutlook:this.formInline.politics,
          //招聘计划
          recruitmentPlanId:this.formInline.recruitmentPlanId,


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
    // 修改教育经历
    updateEducations(){
      this.axios({
        url: 'http://localhost:8010/provider/educationss/updateEducationss',
        method: 'put',
        data: {
          //教育经历id
          educationssId: this.formInline.educationssId,
          //学校名称
          educationStudentname: this.formInline.school,
          //所学专业
          educationMajor: this.formInline.major,
          //开始时间
          resumeBirthdayeducationStartTime: this.formInline.start,
          //结束时间
          educationEndTime: this.formInline.end,
          //是否全日制
          educationFullTime:this.formInline.full_time,
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
    // 修改工作经历
    updateWorks(){
      this.axios({
        url: 'http://localhost:8010/provider/workExperiencess/updateWork',
        method: 'put',
        data: {
          //工作经历id
          workExperiencessId: this.formInline.workExperiencessId,
          //公司名称
          companyName: this.formInline.company,
          // 开始时间
          workStareTime: this.formInline.start1,
          //结束时间
          workEndTime: this.formInline.end1,
          //所属行业
          positionIndustry: this.formInline.industry,
          //税前月薪
          positionSqmonthly:this.formInline.salary,
          //工作描述
          positionDescribe: this.formInline.describe,
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
    },*/
    //取值
    reVal(){

      //简历表基本信息
      let resumes={
        //简历id
        resumeId: this.formInline.resumeId,
        // 姓名
        resumeName: this.formInline.name,
        //性别
        resumeSex: this.formInline.sex,
        // 出生日期
        resumeBirthday: this.formInline.day,
        //手机号
        resumePhone: this.formInline.phone,
        //年龄
        resumeAge: this.formInline.adg,
        //邮箱
        resumeMailbox: this.formInline.mailbox,
        // 所在地
        resumeResidence: this.formInline.Location,
        //毕业学校
        resumeScoller:this.formInline.byschool,
        //学历
        resumeEducation: this.formInline.education,
        //政治面貌
        resumePoliticalOutlook: this.formInline.politics,
        //招聘计划
        recruitmentPlanId: this.formInline.recruitmentPlanId,
      }
      let educations={
        //教育经历id
        educationssId: this.formInline.educationssId,
        // 学校名称
        educationStudentname: this.formInline.school,
        //所学专业
        educationMajor: this.formInline.major,
        // 开始时间
        educationStartTime: this.formInline.start,
        //结束时间
        educationEndTime: this.formInline.end,
        //是否全日制
        educationFullTime: this.formInline.full_time,
      }
      let work={
        //工作经历id
        workExperiencessId: this.formInline.workExperiencessId,
        // 公司名称
        companyName: this.formInline.company,
        //职位名称
        positionName: this.formInline.position,
        // 开始时间
        workStareTime: this.formInline.start1,
        //结束时间
        workEndTime: this.formInline.end1,
        //所属行业
        positionIndustry: this.formInline.industry,
        //税前月薪
        positionSqmonthly: this.formInline.salary,
        //工作描述
        positionDescribe: this.formInline.describe,

      }
      if(this.$route.query.name=="修改"){
        this.updateResume(resumes,educations,work)
        return
      }
      this.insertResume(resumes,educations,work)

    },

    //添加简历
    insertResume(resumes,educations,work){
      this.axios({
        url: 'http://localhost:8010/provider/resume/resumes',
        method: 'post',
        data:{
          Resume:resumes,
          WorkExperiencess:work,
          Educationss:educations
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.$router.go('-1');
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //添加简历
    updateResume(resumes,educations,work){
      this.axios({
        url: 'http://localhost:8010/provider/resume/updateResume',
        method: 'put',
        data:{
          Resume:resumes,
          WorkExperiencess:work,
          Educationss:educations
        }
      }).then(response => {
        if (response.data.data > 0) {
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

    //根据id查简历
    selectResumes(){
      this.axios
          .get("http://localhost:8010/provider/resume/selectResumes/" + this.$route.query.id)
          .then((response) => {
            console.log("1111111111")
            console.log(response);
            this.resumesMsg = response.data.data;
            this.formInline.resumeId=this.resumesMsg.resumeId
            //姓名
            this.formInline.name=this.resumesMsg.resumeName
            //性别
            this.formInline.sex=this.resumesMsg.resumeSex
            // 出生日期
            this.formInline.day=this.resumesMsg.resumeBirthday
            //年龄
            this.formInline.adg= this.resumesMsg.resumeAge
            //毕业学校
            this.formInline.byschool=this.resumesMsg.resumeScoller
            //手机号
            this.formInline.phone=this.resumesMsg.resumePhone
            //邮箱
            this.formInline.mailbox=this.resumesMsg.resumeMailbox
            //所在地
            this.formInline.Location=this.resumesMsg.resumeResidence
            //学历
            this.formInline.education=this.resumesMsg.resumeEducation
            //政治面貌
            this.formInline.politics=this.resumesMsg.resumePoliticalOutlook
            //招聘计划
            this.formInline.recruitmentPlanId=this.resumesMsg.recruitmentPlanId
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //根据id查教育经历
    selectEducationss(){
      this.axios
          .get("http://localhost:8010/provider/educationss/selectEducationss/" + this.$route.query.id)
          .then((response) => {
            console.log("222222222")
            console.log(response);
            this.educationssMsg = response.data.data;
            this.formInline.educationssId=this.educationssMsg.educationssId
            //学校名称
            this.formInline.school=this.educationssMsg.educationStudentname
            //所学专业
            this.formInline.major=this.educationssMsg.educationMajor
            //开始时间
            this.formInline.start=this.educationssMsg.educationStartTime
            //结束时间
            this.formInline.end= this.educationssMsg.educationEndTime
            //是否全日制
            this.formInline.full_time=this.educationssMsg.educationFullTime

          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //根据id查工作经历
    selectWork(){
      this.axios
          .get("http://localhost:8010/provider/workExperiencess/selectWork/" + this.$route.query.id)
          .then((response) => {
            console.log("3333333333")
            console.log(response);
            this.workMsg = response.data.data;
            this.formInline.workExperiencessId=this.workMsg.workExperiencessId
            //公司名称
            this.formInline.company=this.workMsg.companyName
            //职位名称
            this.formInline.position=this.workMsg.positionName
            // 开始时间
            this.formInline.start1=this.workMsg.workStareTime
            //结束时间
            this.formInline.end1= this.workMsg.workEndTime
            //所属行业
            this.formInline.industry=this.workMsg.positionIndustry
            //税前月薪
            this.formInline.salary=this.workMsg.positionSqmonthly
            //工作描述
            this.formInline.describe=this.workMsg.positionDescribe
          })
          .catch(function (error) {
            console.log(error);
          });

    },


    uploadImg(e) {
      let _this = this;
      let files = e.target.files[0];
      if (!e || !window.FileReader) return; // 看支持不支持FileReader
      let reader = new FileReader();
      reader.readAsDataURL(files); // 这里是最关键的一步，转换就在这里
      reader.onloadend = function () {
        _this.src = this.result;
        _this.isShow = true;
      }
    },
    //查询招聘计划名称
    selectzpjh() {
      this.axios
          .get("http://localhost:8010/provider/recruitmentPlan/selectzpjh")
          .then((response) => {
            console.log(response);
            this.zpjhlists = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    forkImage() {
      this.src = '';
      this.isShow = false;
    },


    //取消按钮事件：返回上一级
    goblack(){
      this.$router.go('-1');
    },
    resetForm(formName,formName1,formName2) {
      this.$refs[formName].resetFields();
      this.$refs[formName1].resetFields();
      this.$refs[formName2].resetFields();

    },
    submitForm(formName,formName1,formName2) {

      let submits=0

      this.$refs[formName].validate((valid) => {
        if (valid) {
          submits++
        } else {
          console.log('error submit!!');
          return false;
        }
      });

      this.$refs[formName1].validate((valid) => {
        if (valid) {
          submits++
        } else {
          console.log('error submit!!');
          return false;
        }
      });

      this.$refs[formName2].validate((valid) => {
        if (valid) {
          submits++
        } else {
          console.log('error submit!!');
          return false;
        }
      });

      if (submits==3) this.reVal()



    },

  },created() {
    this.selectzpjh()
    if(this.$route.query.name=="修改"){
      this.selectResumes()
      this.selectEducationss()
      this.selectWork()
    }


  }

}


</script>


<style type="text/css" scoped>
/deep/ .el-form-item__content {
  padding: 0 35px 0 0;
}


.common {
  width: 170px;
  height: 170px;
  border: 1px solid #d8d8d9;
}

.img-list-item {
  position: relative;
}

.img-list-item img {
  box-sizing: border-box;
  vertical-align: middle;
  border: 0;
}

.img-list-item i.del-img {
  width: 15px;
  height: 15px;
  display: inline-block;
  background: rgba(0, 0, 0, .6);
  background-size: 10px;
  background-repeat: no-repeat;
  background-position: 50%;
  position: absolute;
  top: 0;
  right: 0;
}

input[type="file"] {
  color: transparent;
  opacity: 0;
  width: 100%;
  height: 100%;
}

#wrapper:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}

.mb_10 {
  margin-bottom: 10px;
  margin-top: 35px;
}

.j-card-head {
  height: 48px;
  line-height: 48px;
  background: #fff;
  border-bottom: 1px solid #e9e9e9;
  margin: 0 24px;
  overflow: hidden;
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
