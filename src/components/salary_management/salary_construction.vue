<!--薪酬结构 -->
<template>
  <div class="demo-collapse">
    <div style="width: 95%;margin: auto">
    <div class="">
<!--      <span style="font-size: 14px;">1、选择一个部门</span>&nbsp;-->
      <!--选择部门的下拉框-->
      <el-form>
      <el-form-item label="1、选择一个薪酬组：" prop="dept">
        <el-select v-model="ruleForm.compensationName" @change="selectWorkschemeId()" placeholder="请选择一个薪酬组" style="width: 200px;margin-left: 15px">
          <el-option
              v-for="item in CompensationNameAll"
              :key="item.compensationId"
              :label="item.compensationName"
              :value="item.compensationId"
          >
          </el-option>
        </el-select>
      </el-form-item>
      </el-form><br>
<!--      <p class="font_size_p">2.根据您企业薪酬制度，选择需要的薪酬类别（勾选则表示系统需核算）</p>-->

<!--      <br>-->

<!--      <el-checkbox-group v-model="checkList" >-->
<!--        <el-tooltip class="item" effect="dark" content="固定员工核算基础数据,不可更改" placement="top-start">-->
<!--          <el-icon :size="16" style="margin-left: 20px;">-->
<!--            <i-question-filled />-->
<!--          </el-icon>-->

<!--        </el-tooltip>-->
<!--        <el-checkbox label="固定工资" disabled  style=" margin-left: 20px;"/>-->
<!--        <el-checkbox label="加班工资" disabled style=" margin-left: 20px;"/>-->
<!--        <el-checkbox label="考勤扣款" style=" margin-left: 20px;"/>-->
<!--        <el-checkbox label="社保" style=" margin-left: 20px;"/>-->
<!--        <el-checkbox label="公积金" style=" margin-left: 20px;"/>-->
<!--      </el-checkbox-group>-->
    </div>
    <br />
    <div class="">

      <span class="font_size_p">2、设置各薪酬类别下面的薪资项目（开启后在工资表中显示该项目）</span>

      <br>
      <br>
      <div class="">
        <el-collapse v-model="activeNames" @change="handleChange">


          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">
              <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">固定工资</h3>
              <span class="span_1_zhe" >【加项】针对员工定薪、调薪等，可设置固定工资方案  <router-link :to="{path:this.regular,query:{path: this.$route.query.path}}">去设置</router-link> </span>
          </div>
          <br />


          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">
              <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">加班工资</h3>
              <span class="span_1_zhe" >【加项】针对员工工作日、休息日、节假日加班工资，可设置加班工资方案  <router-link :to="{path:this.callbackpay,query:{path: this.$route.query.path}}">去设置</router-link> </span>

<!--            <el-input  style="width:200px;margin-left: 150px;"></el-input>-->
            <el-form style="display: inline-block;margin-left: 235px;">
              <el-form-item label="加班方案：" prop="dept">
                <el-select v-model="overtimeForm.workschemeId" placeholder="请选择一个加班方案" style="width: 200px;">
                  <el-option
                      v-for="item in overtimeAll"
                      :key="item.workschemeId"
                      :label="item.workschemeName"
                      :value="item.workschemeId"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
            </el-form>

          </div>
          <br />


          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">
              <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">考勤扣款</h3>
              <span class="span_1_zhe" >【减项】该部分工资，系统根据考勤扣款方案自动核算数据，可针对每项设置扣款方案  <router-link :to="{path:this.attendanceplan,query:{path: this.$route.query.path}}">去设置</router-link> </span>

            <el-form style="display: inline-block;margin-left: 165px;">
              <el-form-item label="考勤方案：" prop="dept">
                <el-select v-model="attendanceForm.attendandceId"  placeholder="请选择一个考勤方案" style="width: 200px;">
                  <el-option
                      v-for="item in attendanceAll"
                      :key="item.attendandceId"
                      :label="item.attendandceName"
                      :value="item.attendandceId"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
            </el-form>

          </div>

          <br/>

          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">
              <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">出差工资</h3>
              <span class="span_1_zhe" >【加项】针对员工出差，可设置出差方案 <router-link :to="{path:this.evectionplan,query:{path: this.$route.query.path}}">去设置</router-link> </span>

            <el-form style="display: inline-block;margin-left: 445px;">
              <el-form-item label="出差方案：" prop="dept">
                <el-select v-model="evectionForm.businessId"  placeholder="请选择一个出差方案" style="width: 200px;">
                  <el-option
                      v-for="item in evectionAll"
                      :key="item.businessId"
                      :label="item.businessName"
                      :value="item.businessId"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
            </el-form>

          </div>
          <br/>
          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">
            <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">社保</h3>
            <span class="span_1_zhe" style="margin-left:155px">【减项】针对员工社保缴纳，在自主社保模块设置数据 <router-link :to="{path:this.evectionplan,query:{path: this.$route.query.path}}">去设置</router-link> </span>
          </div>

<!--          <div style="width:100%;height:50px;border:#D1DBE5 solid 1px;background: #e8edf2">-->
<!--            <h3 style="margin-left: 20px;display: inline-block;margin-top: 10px;">公积金</h3>-->
<!--            <span class="span_1_zhe" style="margin-left:140px">【减项】针对员工公积金缴纳，在自主社保模块设置数据 <router-link :to="{path:this.evectionplan,query:{path: this.$route.query.path}}">去设置</router-link> </span>-->
<!--          </div>-->
          <el-button style="width:150px;margin-top: 30px;margin-left:400px" >取消</el-button>
          <el-button type="primary" style="width:150px" @click="updateCompensation()">确定</el-button>
          <br/>

        </el-collapse>
      </div>
    </div>
    </div>
  </div>

</template>

<script>
import {
  defineComponent,
  ref
} from 'vue'
import {ElMessage} from "element-plus";

export default defineComponent({
  data() {
    return {
      //固定工资
      regular:'/salary/fixed_navigation',
      //加班工资
      callbackpay:'/salary/callbackpay',
      //考勤扣款
      attendanceplan:'/salary/attendanceplan',
      //出差方案
      evectionplan:'/salary/evectionplan',

      checkList:'',
      activeNames:'',
      //存薪酬组名称
      CompensationNameAll:[],
      ruleForm:{
        compensationId:'',
        compensationName:'',
      },

      //存加班方案名称
      overtimeAll:[],
      //加班方案
      overtimeForm:{
        workschemeId:'',
        workschemeName:'',
      },
      //存考勤扣款方案
      attendanceAll:[],
      //考勤扣款
      attendanceForm:{
        attendandceId:'',
        attendandceName:'',
      },

      //存出差方案
      evectionAll:[],
      //出差
      evectionForm:{
        businessId:'',
        businessName:'',
      }

    }
  },methods:{
    //修改薪酬组数据
    updateCompensation(){

      this.axios({
        url: 'http://localhost:8010/provider/compensation/updateCompensation',
        method: 'put',
        data: {
          //薪酬组id
          compensationId:this.ruleForm.compensationName,
          //加班工资方案id
          workschemeId:this.overtimeForm.workschemeId,
          //考勤扣款方案id
          attendandceId:this.attendanceForm.attendandceId,
         //出差方案id
          businessId:this.evectionForm.businessId
        }
      }).then(response => {
        console.log(response)
        if (response.data.data >0) {
          ElMessage({
            message: '修改成功',
            type: 'success',
          })

        } else {
          ElMessage.error('修改失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },

      //查询薪酬组名称
    selectCompensationName() {
      this.axios
          .get("http://localhost:8010/provider/compensation/selectCompensationName")
          .then((response) => {
            console.log(response);
            this.CompensationNameAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //查询出差方案名称
    selectBusinessName() {
      this.axios
          .get("http://localhost:8010/provider/business/selectBusinessName")
          .then((response) => {
            console.log(response);
            this.evectionAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //查询考勤扣款方案名称
    selectAttendandceName() {
      this.axios
          .get("http://localhost:8010/provider/attendandce/selectAttendandceName")
          .then((response) => {
            console.log(response);
            this.attendanceAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //查询加班方案名称
    selectWorkschemeName() {
      this.axios
          .get("http://localhost:8010/provider/workscheme/selectWorkschemeName")
          .then((response) => {
            console.log(response);
            this.overtimeAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //根据id查询薪酬组数据
    //根据id查询加班方案信息
    selectWorkschemeId() {

      this.axios
          .get("http://localhost:8010/provider/compensation/selectCompensationId/" + this.ruleForm.compensationName)
          .then((response) => {
            console.log(response);
            this.overtimeForm.workschemeId=response.data.data.workschemeId
            this.attendanceForm.attendandceId=response.data.data.attendandceId
            this.evectionForm.businessId=response.data.data.businessId
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectCompensationName()
    this.selectBusinessName()
    this.selectWorkschemeName()
    this.selectAttendandceName()
  },
  setup() {
    const checkList = ref(['selected and disabled', 'Option A'])
    const activeNames = ref(['1'])
    const handleChange = (val) => {
      console.log(val)
    }

    return {
      checkList,
      activeNames,
      handleChange,
    }

  },

})
</script>

<style scoped>
@import url("../../css/navigation.css");
@import url("../../css/Salary.css");
/deep/.body_1 {
  background: darkcyan;
  width:100%;
  height: 200px;
  border-radius: 4px;
  /* font-size: 14px; */
  /*position: relative;*/
  /*overflow: hidden;*/
  /*transition: all 0.3s;*/
  /*min-height: 100%;*/
  /*min-height: 720px;*/
  /*padding: 16px;*/
  /*box-sizing: border-box;*/
  /*border: 1px solid #e9e9e9;*/

  /*padding: 0 24px;*/
  /*color: rgba(0, 0, 0, 0.65);*/
}

.demo-collapse{
  width: 100%;
  /*background: #0c9076;*/
  padding-bottom: 30px;
}


/deep/.font_size_p {
  font-size: 14px;
}

/deep/.span_1_zhe {
  color: rgb(153, 153, 153);
  margin-left: 120px;
  display: inline-block;
}

/deep/.ant-row {
  line-height: 22px;
  padding: 12px 0 12px 40px;
}

/deep/.el-collapse-item__header {
  display: flex;
  align-items: center;
  height: var(--el-collapse-header-height);
  line-height: var(--el-collapse-header-height);
  /*background-color: var(--el-collapse-header-bg-color);*/
  /*color: var(--el-collapse-header-text-color);*/
  cursor: pointer;
  /*border-bottom: 1px solid var(--el-collapse-border-color);*/
  font-size: var(--el-collapse-header-font-size);
  font-weight: 500;
  transition: border-bottom-color var(--el-transition-duration);
  outline: 0;
  background-color: #e8edf2;
  border:#D1DBE5 solid 1px ;


}
/deep/.el-collapse-item__content{
  border:#D1DBE5 solid 1px ;
  background-color: #e8edf2;

}

</style>
