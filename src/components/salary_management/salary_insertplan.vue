<!--新增编辑加班方案 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-head">
        <div class="j-card-head-title" style="margin-left:20px;">
          <span>{{this.$route.query.name}}加班方案</span>
        </div>
      </div>
      <div class="j-card-body">
        <div class="sub-Content__primary edit__forms">
          <div class="ant-spin-nested-loading">
            <div class="ant-spin-container">
        <div style="margin-left: 150px">
          <el-form
              ref="ruleForm"
              :model="WorkschemeForm"
              :rules="rules"
              label-width="150px"
              class="demo-ruleForm"
          >
            <el-form-item label="方案名称" prop="schemename" style="width:370px">
              <el-input v-model="WorkschemeForm.schemename" style="width: 207px;"></el-input>
            </el-form-item>

            <el-form-item label="工作日加班工资" >
<!--              <el-select v-model="WorkschemeForm.workday" placeholder="请选择">-->
<!--                <el-option label="按小时工资百分比计薪" style="margin-left: 20px;" value="workbyhour"></el-option>-->
<!--                <el-option label="按固定金额" value="workbyfixed" style="margin-left: 20px;"></el-option>-->
<!--              </el-select>-->
              <el-input v-model="WorkschemeForm.workday" style="width:210px" value=""></el-input>
            </el-form-item>

            <el-form-item label="发放：小时工资 X">
                <el-input-number
                     v-model="num"
                     :min="1"
                     :max="1000"
                     controls-position="right"
                     @change="handleChange"
                     size="small">
                </el-input-number>
              <span>% x 工作日加班折算的小时数</span>
            </el-form-item>


            <el-form-item label="休息日加班工资" >
<!--              <el-select v-model="WorkschemeForm.offday" placeholder="请选择">-->
<!--                <el-option label="按小时工资百分比计薪" value="offbyhour" style="margin-left: 20px;"></el-option>-->
<!--                <el-option label="按固定金额" value="offbyfixed" style="margin-left: 20px;"></el-option>-->
<!--              </el-select>-->
              <el-input v-model="WorkschemeForm.offday" style="width:210px" value=""></el-input>
            </el-form-item>

            <el-form-item label="发放：小时工资 X">
              <el-input-number
                  v-model="num2"
                  :min="1"
                  :max="1000"
                  controls-position="right"
                  @change="handleChange"
                  size="small">
              </el-input-number>
              <span>% x 休息日加班折算的小时数</span>
            </el-form-item>


            <el-form-item label="节假日加班工资" >
<!--              <el-select v-model="WorkschemeForm.holiday" placeholder="请选择">-->
<!--                <el-option label="按小时工资百分比计薪" value="holibyhour" style="margin-left: 20px;"></el-option>-->
<!--                <el-option label="按固定金额" value="holibyfixed" style="margin-left: 20px;"></el-option>-->
<!--              </el-select>-->
              <el-input v-model="WorkschemeForm.holiday" style="width:210px" value=""></el-input>
            </el-form-item>

            <el-form-item label="发放：小时工资 X">
              <el-input-number
                  v-model="num3"
                  :min="1"
                  :max="1000"
                  controls-position="right"
                  @change="handleChange"
                  size="small">
              </el-input-number>
              <span>% x 节假日加班折算的小时数</span>
            </el-form-item>



            <el-form-item label="备注" prop="remark" style="width:500px">
              <el-input v-model="WorkschemeForm.remark" type="textarea"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" style="width: 60px;" @click="submitForm('ruleForm')"
              >提交</el-button
              >
              <el-button style="width: 60px;" @click="goblack()">取消</el-button>
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
import { ElMessage } from 'element-plus'
export default {
  data() {

    return {
      WorkschemeForm: {
        //加班方案名称
        schemename: '',
        //工作日加班工资
        workday: '按小时工资百分比计薪',
        //休息日加班工资
        offday: '按小时工资百分比计薪',
        //节假日加班工资
        holiday: '按小时工资百分比计薪',
        //适用对象
        suitableusers: '',
        //职位
        post: '',
        //备注
        remark: '',
      },

      //存加班方案信息
      workschemeMsg: {},
      //工作日
      num: '150',
      //节假日
      num2: '200',
      //休息日
      num3: '300',
      rules: {
        schemename:[
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
          if(this.$route.query.name=='新增'){
            this.insertWorkscheme();
          }else if(this.$route.query.name=='编辑'){
            this.updateWorkscheme();

          }

        } else {
          console.log('error submit!!')

        }
      })
    },
    //添加加班方案
    insertWorkscheme(){
      alert(this.WorkschemeForm.schemename)
      this.axios({
        url: 'http://localhost:8010/provider/workscheme/insertWorkscheme',
        method: 'post',
        data:{
          //加班方案名称
          workschemeName:this.WorkschemeForm.schemename,
          //节假日加班比例
          workschemeHolidayratio:this.num2,
          //休息日加班比例
          workschemeDayoffratio:this.num3,
          //工作日加班比例
          workschemeWorkratio:this.num,
          //备注
          workschemeRemark:this.WorkschemeForm.remark,
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
    //根据id查询加班方案信息
    selectWorkschemeId() {
      alert(this.$route.query.id)
      this.axios
          .get("http://localhost:8010/provider/workscheme/selectWorkschemeId/" + this.$route.query.id)
          .then((response) => {
            console.log(response);
            this.workschemeMsg = response.data.data;

            this.WorkschemeForm.schemename=this.workschemeMsg.workschemeName;
            this.num=this.workschemeMsg.workschemeWorkratio;
            this.num2=this.workschemeMsg.workschemeHolidayratio;
            this.num3=this.workschemeMsg.workschemeDayoffratio;
            this.WorkschemeForm.remark=this.workschemeMsg.workschemeRemark;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //修改加班方案
    updateWorkscheme(){

      this.axios({
        url: 'http://localhost:8010/provider/workscheme/updateWorkscheme',
        method: 'put',
        data: {
          //加班方案id
          workschemeId:this.$route.query.id,
          //加班方案名称
          workschemeName:this.WorkschemeForm.schemename,
          //节假日加班比例
          workschemeHolidayratio:this.num2,
          //休息日加班比例
          workschemeDayoffratio:this.num3,
          //工作日加班比例
          workschemeWorkratio:this.num,
          //备注
          workschemeRemark:this.WorkschemeForm.remark,
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
      this.selectWorkschemeId()
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