<!--新增编辑考勤方案 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-head">
        <div class="j-card-head-title" style="margin-left:20px;">
          <span>{{this.$route.query.name}}考勤方案</span>
        </div>
      </div>
      <div class="j-card-body">
        <div class="sub-Content__primary edit__forms">
          <div class="ant-spin-nested-loading">
            <div class="ant-spin-container">
              <div style="margin-left: 150px">
                <el-form
                    ref="ruleForm"
                    :model="attendandceForm"
                    :rules="rules"
                    label-width="150px"
                    class="demo-ruleForm"
                >
                  <el-form-item label="方案名称：" prop="attendandceName" style="width:370px">
                    <el-input v-model="attendandceForm.attendandceName" style="width: 207px"></el-input>
                  </el-form-item>



                  <el-form-item label="迟到：" prop="late">
<!--                    <el-select v-model="ruleForm.late" placeholder="请选择">-->
<!--                      <el-option label="按固定金额扣款" value="latebyfixed" style="margin-left: 20px;"></el-option>-->
<!--                      <el-option label="按迟到时长扣款" value="latebyhour" style="margin-left: 20px;"></el-option>-->
<!--                      <el-option label="按迟到次数扣款" value="latebynumber" style="margin-left: 20px;"></el-option>-->
<!--                    </el-select>-->
                    <el-input v-model="attendandceForm.late" style="width:210px" value=""></el-input>

                  </el-form-item>

                  <el-form-item label="扣款："  style="width:500px">
                    <el-input-number
                        v-model="num"
                        :min="0"
                        :max="10000"
                        controls-position="right"
                        @change="handleChange"
                        size="small"
                    />
                    <span>元x迟到次数</span>
                  </el-form-item>



                  <el-form-item label="早退：" prop="early">
<!--                    <el-select v-model="ruleForm.early" placeholder="请选择">-->
<!--                      <el-option label="按固定金额扣款" value="earlybyfixed" style="margin-left: 20px;"></el-option>-->
<!--                      <el-option label="按早退时长扣款" value="earlybyhour" style="margin-left: 20px;"></el-option>-->
<!--                      <el-option label="按早退次数扣款" value="earlybynumber" style="margin-left: 20px;"></el-option>-->
<!--                    </el-select>-->
                    <el-input v-model="attendandceForm.early" style="width:210px" value=""></el-input>

                  </el-form-item>


                  <el-form-item label="扣款："  style="width:500px">
                    <el-input-number
                        v-model="num2"
                        :min="0"
                        :max="10000"
                        controls-position="right"
                        @change="handleChange"
                        size="small"
                    />
                    <span>元x早退次数</span>
                  </el-form-item>




<!--                  <el-form-item label="未签到：" prop="notsignin">-->

<!--                    <el-input v-model="attendandceForm.notsignin" style="width:210px" value=""></el-input>-->

<!--                  </el-form-item>-->



<!--                  <el-form-item label="扣款："  style="width:500px">-->
<!--                    <el-input-number-->
<!--                        v-model="num3"-->
<!--                        :min="0"-->
<!--                        :max="10000"-->
<!--                        controls-position="right"-->
<!--                        @change="handleChange"-->
<!--                        size="small"-->
<!--                    />-->
<!--                    <span>元x未签到次数</span>-->
<!--                  </el-form-item>-->



<!--                  <el-form-item label="未签退：" prop="notsignback">-->

<!--                    <el-input v-model="attendandceForm.notsignback" style="width:210px" value=""></el-input>-->

<!--                  </el-form-item>-->


<!--                  <el-form-item label="扣款："  style="width:500px">-->
<!--                    <el-input-number-->
<!--                        v-model="num4"-->
<!--                        :min="0"-->
<!--                        :max="10000"-->
<!--                        controls-position="right"-->
<!--                        @change="handleChange"-->
<!--                        size="small"-->
<!--                    />-->
<!--                    <span>元x未签退次数</span>-->
<!--                  </el-form-item>-->




                  <el-form-item label="旷工：" prop="absent">

                    <el-input v-model="attendandceForm.absent" style="width:210px" value=""></el-input>

                  </el-form-item>

                  <el-form-item label="扣款："  style="width:500px">
                    <el-input-number
                        v-model="num5"
                        :min="1"
                        :max="10000"
                        controls-position="right"
                        @change="handleChange"
                        size="small"
                    />
                    <span>元 × 旷工折算的小时数</span>
                  </el-form-item>


                  <el-form-item label="备注" prop="remark" style="width:500px">
                    <el-input v-model="attendandceForm.attendandceRemark" type="textarea"></el-input>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" style="width: 60px;" @click="submitForm('ruleForm')"
                    >提交</el-button
                    >
                    <router-link to="/attendance">&nbsp;
                      <el-button @click="goblack()" style="width: 60px;">取消</el-button></router-link>
                  </el-form-item>
                </el-form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  &nbsp;
</template>




<script lang="ts">
import {ElMessage} from "element-plus";

export default {
  data() {

    return {
      attendandceForm: {
        //考勤方案名称
        attendandceName: '',

        late: '按迟到次数扣款',
        early: '按早退次数扣款',
        notsignin: '按未签到次数扣款',
        notsignback: '按未签退次数扣款',
        absent: '按小时工资比例扣款',
        //
        attendandceRemark: '',
      },
      //存考勤扣款信息
      AttendandceMsg:[],
      //迟到金额
      num: '0',
      //早退金额
      num2:'0',
      //未签到
      num3:'0',
      //未签退
      num4:'0',
      //旷工
      num5:'300',
      rules: {
        attendandceName:[
          {
            required: true,
            message: '请输入方案名称',
            trigger: 'blur',
          }
        ],

      }
    }
  },
  methods:{
    goblack(){
      this.$router.go('-1');
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if(this.$route.query.name='新增'){
            this.insertAttendandce();
          }else{
            this. updateAttendandce()
          }


        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    //新增考勤扣款方案
    insertAttendandce(){
      alert(this.attendandceForm.attendandceName)
      this.axios({
        url: 'http://localhost:8010/provider/attendandce/insertAttendandce',
        method: 'post',
        data:{

          // 考勤扣款方案名称
          attendandceName:this.attendandceForm.attendandceName,
          //迟到一次金额
          attendandceLitemoney:this.num,
          //早退一次金额
          attendandceLeavemoney:this.num2,
          // //未签到一次金额
          // attendandceDidnotmoney:this.num3,
          // //未签退一次金额
          // attendandceDidbackmoney:this.num4,
          //旷工一天金额
          attendandceAbscntmoney:this.num5,
          //备注
          attendandceRemark:this.attendandceForm.attendandceRemark,
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
    //根据id查询考勤扣款方案信息
    selectAttendandceId() {
      alert(this.$route.query.id)
      this.axios
          .get("http://localhost:8010/provider/attendandce/selectAttendandceId/" + this.$route.query.id)
          .then((response) => {
            console.log(response);
            this.AttendandceMsg = response.data.data;

            // 考勤扣款方案名称
            this.attendandceForm.attendandceName=this.AttendandceMsg.attendandceName,
                //迟到一次金额
                this.num=this.AttendandceMsg.attendandceLitemoney,
                //早退一次金额
                this.num2=this.AttendandceMsg.attendandceLeavemoney,
                // //未签到一次金额
                // this.num3=this.AttendandceMsg.attendandceDidnotmoney,
                // //未签退一次金额
                // this.num4=this.AttendandceMsg.attendandceDidbackmoney,
                //旷工一天金额
                this.num5=this.AttendandceMsg.attendandceAbscntmoney,
                //备注
                this.attendandceForm.attendandceRemark=this.AttendandceMsg.attendandceRemark
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //修改考勤扣款信息
    updateAttendandce(){
      this.axios({
        url: 'http://localhost:8010/provider/attendandce/updateAttendandce',
        method: 'put',
        data: {
          //出差方案id
          businessId:this.$route.query.id,
          // 考勤扣款方案名称
          attendandceName:this.attendandceForm.attendandceName,
          //迟到一次金额
          attendandceLitemoney:this.num,
          //早退一次金额
          attendandceLeavemoney:this.num2,
          // //未签到一次金额
          // attendandceDidnotmoney:this.num3,
          // //未签退一次金额
          // attendandceDidbackmoney:this.num4,
          //旷工一天金额
          attendandceAbscntmoney:this.num5,
          //备注
          attendandceRemark:this.attendandceForm.attendandceRemark,
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
    }
  },created() {
    if(this.$route.query.name=='编辑'){
      this.selectAttendandceId()
    }
  }
}
</script>


<style scoped>
@import url("../../css/navigation.css");
@import url(../../css/dimission.css);

/deep/.el-form-item__label {
  flex: 0 0 auto;
  text-align: right;
  font-size: var(--el-form-label-font-size);
  color: var(--el-text-color-regular);
  line-height: 40px;
  padding: 0 12px 0 0;
  box-sizing: border-box;
  width: 240px;
}
</style>