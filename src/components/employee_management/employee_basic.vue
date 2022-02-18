<!-- 员工编辑基本档案-->
<template>


  <div class="basicfile_big">
    <div class="basicfile_big_auto">
      <div style="border-left:3px solid blue;">
        <h3>
          <span style="margin-left:10px;">基本档案</span>
        </h3>
      </div>


      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">基本信息</h3>
        <div style="width:860px;border-top:1px solid silver;display: inline-block;margin-left: 20px;margin-bottom: 5px;"></div>
        <h3 @click="informations_1=!informations_1,informations_edit_1=!informations_edit_1,informations_bj_1=!informations_bj_1" v-show="informations_bj_1" style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;"><i class="iconfont">&#xe600;</i>编辑</h3>
      </div>



      <div style="width: 100%;margin-top: 20px;">
        <div class="information_text" v-show="informations_1">
          <ul style="list-style-type: none; ">
            <li>
              <label>员工编号</label>
              <p>{{ empMsg.staffId }}</p>
            </li>
            <li>
              <label>姓名</label>
              <p>{{empMsg.staffName}}</p>
            </li>
            <li>
              <label>出生日期</label>
              <p>{{empMsg.staffBirthday}}</p>
            </li>
            <li>
              <label>年龄</label>
              <p>{{empMsg.staffAge}}</p>
            </li>
            <li>
              <label>婚姻状况</label>
              <p>{{empMsg.staffMarital}}</p>
            </li>
            <li>
              <label>最高学历</label>
              <p>{{empMsg.staffEducation}}</p>
            </li>
            <li>
              <label>身份证</label>
              <p>{{empMsg.staffIdentity}}</p>
            </li>
            <li>
              <label>性别</label>
              <p>{{empMsg.staffSex}}</p>
            </li>
            <li>
              <label>政治面貌</label>
              <p>{{empMsg.staffOutlook}}</p>
            </li>
            <li>
              <label>户口所在地</label>
              <p>{{empMsg.staffRegistered}}</p>
            </li>
            <li>
              <label>毕业学校</label>
              <p>{{empMsg.staffSchool}}</p>
            </li>
            <li>
              <label>银行卡号</label>
              <p>{{empMsg.staffCredit}}</p>
            </li>
            <li>
              <label>血型</label>
              <p>{{empMsg.staffBlood}}</p>
            </li>
            <li>
              <label>星座</label>
              <p>{{empMsg.staffSign}}</p>
            </li>
            <li>
              <label>专业</label>
              <p>{{empMsg.staffMajor}}</p>
            </li>


          </ul>
        </div>


        <div class="information_from" v-show="informations_edit_1">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="essentialForm" :model="empMsg">

            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px;text-align: right">
              <el-form-item label="员工编号：" prop="emp_id" >
                <el-input v-model="empMsg.staffId" style="width:240px;" disabled v></el-input>
              </el-form-item><br/>


              <el-form-item label="出生日期：" >
                <el-col :span="11">
                  <el-form-item prop="birthday" style="width:240px;position: absolute;left:10px">
                    <el-date-picker type="date" placeholder="选择日期" v-model="empMsg.staffBirthday" style="width: 100%;"></el-date-picker>
                  </el-form-item>
                </el-col>
              </el-form-item><br/>

              <el-form-item label="婚姻状态：" prop="marital">
                <el-select style="width:240px;" v-model="empMsg.staffMarital">
                  <el-option label="已婚" value="yh"></el-option>
                  <el-option label="未婚" value="wh"></el-option>
                  <el-option label="离异" value="ly"></el-option>

                </el-select>
              </el-form-item><br/>


              <el-form-item label="身份证：" prop="identity">
                <el-input v-model="empMsg.staffIdentity" style="width:240px"></el-input>
              </el-form-item><br/>

              <el-form-item label="政治面貌：" prop="politics">
                <el-select style="width:240px;" v-model="empMsg.staffOutlook">
                  <el-option label="团员" value="ty"></el-option>
                  <el-option label="党员" value="dy"></el-option>
                  <el-option label="群众" value="qz"></el-option>
                </el-select>
              </el-form-item><br/>

              <el-form-item label="毕业学校：" prop="school">
                <el-input v-model="empMsg.staffSchool" style="width:240px;"></el-input>
              </el-form-item><br/>


              <el-form-item label="血型：" prop="blood">
                <el-input v-model="empMsg.staffBlood" style="width:240px;"></el-input>
              </el-form-item><br/>




              <el-form-item label="专业：" prop="major">
                <el-input v-model="empMsg.staffMajor" style="width:240px;"></el-input>
              </el-form-item>
            </div>



            <div style="display: inline-block;position: absolute;top:20px;right:150px;text-align: right">
              <br/>
              <el-form-item prop="staffName" >
                <template #label >姓名：</template>
                <el-input v-model="empMsg.staffName" style="width:240px;" disabled></el-input>
              </el-form-item><br/>



              <el-form-item label="年龄：" prop="emp_age">
                <el-input v-model="empMsg.staffAge" style="width:240px;"></el-input>
              </el-form-item><br/>



              <el-form-item label="最高学历：" prop="official">
                <el-select v-model="empMsg.staffEducation" placeholder="请选择活动区域" style="width:240px;">
                  <el-option label="硕士" value="硕士"></el-option>
                  <el-option label="本科" value="本科"></el-option>
                  <el-option label="大专" value="大专"></el-option>
                  <el-option label="中专" value="中专"></el-option>
                  <el-option label="高中" value="高中"></el-option>
                  <el-option label="其他" value="其他"></el-option>
                </el-select>
              </el-form-item><br/>


              <el-form-item label="性别：" prop="emp_sex">
                <el-radio-group v-model="empMsg.staffSex" style="position: absolute;left:47px">
                  <el-radio label="男"></el-radio>
                  <el-radio label="女"></el-radio>
                </el-radio-group>
              </el-form-item><br/>



              <el-form-item label="户口所在地：" prop="registered">
                <el-input v-model="empMsg.staffRegistered" style="width:240px;"></el-input>
              </el-form-item><br/>


              <el-form-item label="银行卡号：" prop="credit">
                <el-input v-model="empMsg.staffCredit" style="width:240px;"></el-input>
              </el-form-item><br/>


              <el-form-item label="星座：" prop="constellation">
                <el-input v-model="empMsg.staffSign" style="width:240px;"></el-input>
              </el-form-item>
            </div>



            <div style="width:90%;height:60px;margin: auto;margin-top: 20px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">
                <el-button @click="informations_1=!informations_1,informations_edit_1=!informations_edit_1,informations_bj_1=!informations_bj_1">取消</el-button>
                <el-button type="primary" @click="updateStaff(),selectStaffVoId()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>
      </div>




      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">联系方式</h3>
        <div style="width:860px;border-top:1px solid silver;display: inline-block;margin-left: 20px;margin-bottom: 5px;"></div>
        <h3 @click="informations_2=!informations_2,informations_edit_2=!informations_edit_2,informations_bj_2=!informations_bj_2" v-show="informations_bj_2" style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;"><i class="iconfont">&#xe600;</i>编辑</h3>

      </div>



      <div style="width: 100%;margin-top: 20px;">
        <div class="information_text" v-show="informations_2">
          <ul style="list-style-type: none; ">
            <li>
              <label>手机号</label>
              <p>{{empMsg.staffPhone}}</p>
            </li>
            <li>
              <label>个人邮箱</label>
              <p>{{empMsg.staffEmail}}</p>
            </li>
            <li>
              <label>微信</label>
              <p>{{empMsg.staffWechat}}</p>
            </li>
            <li>
              <label>QQ</label>
              <p>{{empMsg.staffQq}}</p>
            </li>
            <li>
              <label>现住地址</label>
              <p>{{empMsg.staffAddress}}</p>
            </li>
            <li>
              <label>紧急联系人</label>
              <p>{{empMsg.staffEmergency}}</p>
            </li>

          </ul>
        </div>

        <div class="information_from" v-show="informations_edit_2">
          <el-form style="width: 90%;margin: auto; " :rules="rules" ref="contactForm" :model="empMsg">

            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px;text-align: right">

              <el-form-item label="手机号" prop="phone" >
                <el-input v-model="empMsg.staffPhone" style="width:240px;"></el-input>
              </el-form-item><br/>

              <el-form-item label="微信：" prop="WeChat">
                <el-input v-model="empMsg.staffWechat" style="width:240px"></el-input>
              </el-form-item><br/>


              <el-form-item label="现住地址：" prop="address">
                <el-input v-model="empMsg.staffAddress" style="width:240px;"></el-input>
              </el-form-item><br/>

            </div>



            <div style="display: inline-block;position: absolute;top:20px;right:150px;text-align: right">
              <br/>
              <el-form-item  prop="eamil">
                <template #label >个人邮箱：</template>
                <el-input v-model="empMsg.staffEmail" style="width:240px;"></el-input>
              </el-form-item><br/>


              <el-form-item label="QQ：" prop="qq">
                <el-input v-model="empMsg.staffQq" style="width:240px;"></el-input>
              </el-form-item><br/>


              <el-form-item label="紧急联系人：" prop="emergency">
                <el-input v-model="empMsg.staffEmergency" style="width:240px;"></el-input>
              </el-form-item><br/>


            </div>

            <div style="width:90%;height:60px;margin: auto;margin-top: 20px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">

                <el-button @click="informations_2=!informations_2,informations_edit_2=!informations_edit_2,informations_bj_2=!informations_bj_2">取消</el-button>
                <el-button type="primary" @click="updateStaff()">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>
      </div>


      <div class="information">
        <h3 style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;">在职信息</h3>
        <div style="width:860px;border-top:1px solid silver;display: inline-block;margin-left: 20px;margin-bottom: 5px;"></div>
        <h3 @click="informations_3=!informations_3,informations_edit_3=!informations_edit_3,informations_bj_3=!informations_bj_3" v-show="informations_bj_3" style="color: #085fc3;font-size: 14px;margin-left: 10px;display: inline-block;"><i class="iconfont">&#xe600;</i>编辑</h3>

      </div>



      <div style="width: 100%;margin-top: 20px;">
        <div class="information_text" v-show="informations_3">
          <ul style="list-style-type: none; ">
            <li>
              <label>部门</label>
              <p>{{empMsg.deptName}}</p>
            </li>
            <li>
              <label>职位</label>
              <p>{{empMsg.postName}}</p>
            </li>
            <li>
              <label>员工状态</label>
              <p>{{empMsg.staffState==2?'试用':'正式'}}</p>
            </li>
            <li>
              <label>入职日期</label>
              <p>{{empMsg.staffHiredate}}</p>
            </li>
            <li>
              <label>工龄</label>
              <p>{{empMsg.staffWorkingYears}}</p>
            </li>
            <li>
              <label>转正日期</label>
              <p>{{empMsg.workerDate}}</p>
            </li>

          </ul>
        </div>

        <div class="information_from" v-show="informations_edit_3">
          <el-form style="width: 90%;margin: auto;" ref="informationForm" :model="empMsg">

            <br/>
            <div style="display: inline-block;margin:20px 0px 0px 50px;text-align: right">

<!--              <el-form-item label="部门：" prop="dept">-->
<!--                <el-input v-model="empMsg.deptName"  style="width:240px;"></el-input>-->
<!--              </el-form-item><br/>-->

              <el-form-item label="部门:" prop="dept">
                <el-select v-model="empMsg.deptId" placeholder="请输入部门名称" style="width: 240px;">
                  <el-option
                      v-for="item in deptNameAll"
                      :key="item.deptId"
                      :label="item.deptName"
                      :value="item.deptId"
                  >
                  </el-option>

                </el-select>
              </el-form-item><br/>

              <el-form-item label="职位:" prop="position">
                <el-select v-model="empMsg.postSearch" placeholder="请输入部门名称" style="width: 200px;margin-left: 15px">
                  <el-option
                      v-for="item in positionAll"
                      :key="item.positionId"
                      :label="item.positionName"
                      :value="item.positionId"
                  >
                  </el-option>

                </el-select>
              </el-form-item><br/>


              <el-form-item label="工龄：" prop="service">
                <el-input v-model="empMsg.staffWorkingYears" style="width:240px;" disabled></el-input>
              </el-form-item><br/>

            </div>



            <div style="display: inline-block;position: absolute;top:20px;right:150px;text-align: right">
              <br/>


              <el-form-item label="入职日期：" prop="boardDate">
                <el-date-picker
                    v-model="empMsg.staffHiredate"
                    type="date"
                    placeholder="选择日期" style="width: 240px">
                </el-date-picker>
              </el-form-item><br/>

              <el-form-item label="员工状态：" prop="state">
                <el-radio-group v-model="empMsg.staffState" style="position: absolute;left:4px" disabled>
                  <el-radio :label="2">试用</el-radio>
                  <el-radio :label="3">正式</el-radio>
                </el-radio-group>
              </el-form-item><br/>

              <el-form-item label="转正日期：" prop="positive">
                <el-date-picker
                    v-model="empMsg.workerDate"
                    type="date"
                    placeholder="选择日期" style="width: 240px" disabled>
                </el-date-picker>
              </el-form-item><br/>



            </div>

            <div style="width:90%;height:60px;margin: auto;margin-top: 20px;padding: 0px 0px 30px 0px">
              <div style="width:20%;height:50px;margin:auto;">

                <el-button @click="informations_3=!informations_3,informations_edit_3=!informations_edit_3,informations_bj_3=!informations_bj_3">取消</el-button>
                <el-button @click="updateStaff()" type="primary">保存</el-button>
              </div>
            </div>
          </el-form>
        </div>
      </div>

    </div>
  </div>



</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue'
import { defineComponent, ref } from 'vue'
import {ElMessage} from "element-plus";
export default defineComponent({
  setup() {
    const state = reactive({
      disabledDate(time) {
        return time.getTime() > Date.now()
      },
      shortcuts: [
        {
          text: 'Today',
          value: new Date(),
        },
        {
          text: 'Yesterday',
          value: () => {
            const date = new Date()
            date.setTime(date.getTime() - 3600 * 1000 * 24)
            return date
          },
        },
        {
          text: 'A week ago',
          value: () => {
            const date = new Date()
            date.setTime(date.getTime() - 3600 * 1000 * 24 * 7)
            return date
          },
        },
      ],
      value1: '',
      value2: '',
    })

    return {
      input: ref(''),
      ...toRefs(state),
    }
  },
  data(){
    return{
      // 所有信息
      empMsg:{},
      //部门名称
      deptNameAll:[],
      //部门职位
      positionAll:[],

      rules:{
        name:

            { required: true,
              message: '请输入活动名称',
              trigger: 'blur' },
        id:  { required: true,
          message: '编号不能为空',
          trigger: 'blur' },
        phone: [ { required: true,
          message: '编号不能为空',
          trigger: 'blur' },
          { min: 11,  message: '电话号码要11位数', trigger: 'blur' }
        ]


      },
      informations_1:true,
      informations_edit_1:false,
      informations_bj_1:true,
      informations_2:true,
      informations_edit_2:false,
      informations_bj_2:true,
      informations_3:true,
      informations_edit_3:false,
      informations_bj_3:true
    }

   },
  methods: {

    //根据id查询员工信息
    selectStaffVoId() {
      alert(this.$store.state.staffId_Msg)
      this.axios
          .get("http://localhost:8010/provider/staff/selectStaffId/" + this.$store.state.staffId_Msg)
          .then((response) => {
            console.log(response);
            this.empMsg = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //查询部门名称
    selectDeptName() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectDeptName")
          .then((response) => {
            console.log(response);
            this.deptNameAll = response.data.data;

          })
          .catch(function (error) {
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
    //修改员工信息
    updateStaff(){

      this.axios({
        url: 'http://localhost:8010/provider/staff/updateStaff',
        method: 'put',
         data: this.empMsg
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          //调用查询工作经历
          this.selectStaffVoId()
          //关闭基本信息弹出框
          this.informations_edit_1=false
          //显示员工信息
          this.informations_1=true
          //关闭联系方式弹出框
          this.informations_edit_2=false
          this.informations_2=true
        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    }
  },created() {
    this.selectStaffVoId()
    this.selectDeptName()
    this.selectPositionName()
  }

})
</script>

<style scoped>
/*@import url("../../css/EditingStaff.css");*/
.basicfile_big{
  width:100%;
  /*border:1px solid red;*/
  margin-top:20px;
}
.basicfile_big_auto{
  width:80%;
  /*border:1px solid yellow;*/
  margin: auto;
}
.information{
  margin-top: 20px;
  width:100%;
}

.information_text{
  width:870px;

  margin: auto;
}
.information_text li{
  /*float: left;*/
  display: inline-block;
  width:50%;
  padding: 4px 0px;

}
.information_text li label{
  width: 200px;
  display: inline-block;
  text-align: right;
  vertical-align: middle;
  position: relative;
  color: #969799;
  font-size: 14px;
  padding: 0 24px 0 0;
  box-sizing: border-box;
}
.information_text li p{
  display: inline-block;
}
.iconfont{
  color: #085fc3;
}
.information_from{
  width:100%;
  background: #f4f5f7;
  position: relative;

}
.information_ul li{
  display: inline-block;
  width:50%;
  padding: 20px 64px 20px;
  text-align: right;

}
.information_ul li label{
  /*width:100px;*/
  text-align: right;
  vertical-align: middle;
  position: relative;
  color: rgba(0, 0, 0, 0.85);
  font-size: 14px;
  /*padding: 0 24px 0 0;*/
  box-sizing: border-box;
}

.information_ul{
  list-style-type: none;
  position: relative;
}
.information_input{
  width:240px;
  display: inline-block;
}

</style>