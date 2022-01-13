<!--班次添加-->
<template>
<!--  新增班次页面-->
  <div class="w">
    <div class="he">
      <b>{{this.$route.query.way}}班次</b>
    </div>
    
    <el-form
        ref="ruleForm"
        label-position="top"
        :model="fo"
        :rules="rules"
        label-width="100px"
    >
      <el-form-item prop="classesName" label="班次名称：" style="margin-left: 20px">
        <el-input
            v-model="fo.classesName"
            placeholder="例如：日常班"
            style="width: 250px"
        ></el-input>
      </el-form-item>
      <el-form-item prop="classesBeginDate" label="工作时间：" style="margin-left: 20px">
        <el-time-picker
            v-model="fo.classesBeginDate"
            :disabled-hours="disabledHours"
            :disabled-minutes="disabledMinutes"
            :disabled-seconds="disabledSeconds"
            placeholder="上班时间："
            style="width: 150px"
        >
        </el-time-picker>
        <span>&nbsp;&nbsp;到&nbsp;&nbsp;</span>
        <el-time-picker
            v-model="fo.classesEndDate"
            :disabled-hours="disabledHours"
            :disabled-minutes="disabledMinutes"
            :disabled-seconds="disabledSeconds"
            placeholder="下班时间："
            style="width: 150px"
        >
        </el-time-picker>
      </el-form-item>

      <el-form-item label="班次状态：" style="margin-left: 20px">
      <el-radio v-model="fo.classesState" label="0">启用</el-radio>
      <el-radio v-model="fo.classesState" label="1">禁用</el-radio>
      </el-form-item>


      <el-form-item>
        <div class="u">
          <router-link :to="{path:'/attendance/check',query:{path:this.$route.query.path}}" >
            <el-button>
              <el-icon><i-close-bold /></el-icon>
              <span>取消</span>
            </el-button>
          </router-link>
          <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>
          <el-button type="primary"  @click="submitForm('ruleForm')">
            <el-icon>
              <i-copy-document/>
            </el-icon>
            <span>提交</span>
          </el-button>
        </div>
      </el-form-item>
    </el-form>
  </div>
  <router-view/>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { ElMessage } from 'element-plus'

export default {
  data() {
    return {
      classesState: ref('1'),
      fo: {
        classesId: "",
        classesName: "",
        classesBeginDate: "",
        classesEndDate: "",
        classesState:"1",
      },
      rules: {
        classesName: [
          {
            required: true,
            message: "请填写班次名字",
            trigger: "blur",
          },
        ],
        classesBeginDate: [
          {
            required: true,
            message: "请选择工作时间",
            trigger: "blur",
          }
        ],

      },
    };

  },
  methods: {
 /*   提交验证*/
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {

          if( this.$route.query.way ==="新增" ){
            this.addition();
          }else {
            this.modification();
          }

        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //添加
    addition(){
      this.axios
          .post("http://localhost:8010/provider/classes/add", this.fo)
          .then((response) => {

            if( response.data.data ==="成功" ){
              ElMessage({
                message: '添加成功',
                type: 'success',
              });
              this.$router.go(-1);
            }else{
              ElMessage({
                message: '添加失败',
                type: 'error',
              })
            }


            // console.log(response);
            // this.tableData = response.data.data.records;
            // console.log(response.data.data.records)
          })
          .catch(function (error){
            console.log(error);
          })
    },
    //修改方法
    modification(){
      // 如果 状态为启用 弹出保留一个班次状态
      if(this.$route.query.classesState==0){
        ElMessage.error('请保留一个班次状态')
        return
      }
      this.axios
          .put("http://localhost:8010/provider/classes/amend", this.fo)
          .then((response) => {

            if( response.data.data ==="成功" ){
              ElMessage.success('修改成功')
              this.$router.go(-1);
            }else{
              ElMessage.error('修改失败')
            }
          }).catch(function (error){
        console.log(error);
      })
    },
  },
  //修改取值
  created() {
    if(! this.$route.query.classesName == ""){
      this.fo.classesId=this.$route.query.classesId
      this.fo.classesName=this.$route.query.classesName
      this.fo.classesBeginDate=this.$route.query.classesBeginDate
      this.fo.classesEndDate=this.$route.query.classesEndDate
      this.fo.classesState=this.$route.query.classesState
    }else{
      this.fo.classesName=""
      this.fo.classesBeginDate=""
      this.fo.classesEndDate=""
      this.fo.classesState="1"
    }
  },
};
</script>

<style scoped>
.w {
  margin-top: 20px;
  border: 1px solid #e9e9e9;
}

.w:hover {
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
  border-color: transparent;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

.u {
  margin-left: 40%;
}
.he{
  margin-left: 20px;
  margin-top: 20px;
  font-size: 18px;
}
</style>
