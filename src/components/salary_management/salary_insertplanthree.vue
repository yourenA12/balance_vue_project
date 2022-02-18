<!--新增编辑出差方案 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-head">
        <div class="j-card-head-title" style="margin-left:20px;">
          <span>{{this.$route.query.name}}出差方案</span>
        </div>
      </div>
      <div class="j-card-body">
        <div class="sub-Content__primary edit__forms">
          <div class="ant-spin-nested-loading">
            <div class="ant-spin-container">
              <div style="margin-left: 150px">
                <el-form
                    ref="ruleForm"
                    :model="businessform"
                    :rules="rules"
                    label-width="150px"
                    class="demo-ruleForm"
                >
                  <el-form-item label="方案名称" prop="businessName" style="width:370px">
                    <el-input v-model="businessform.businessName"></el-input>
                  </el-form-item>

                  <el-form-item label="出差工资" prop="businesswage">
<!--                    <el-select v-model="businessform.businesswage" placeholder="请选择">-->
<!--                      <el-option label="按出差时长" value="wagebyhour" style="margin-left: 20px;"></el-option>-->
<!--                      <el-option label="按固定金额" value="wagebyfixed" style="margin-left: 20px;"></el-option>-->
                      <el-input v-model="businessform.businesswage" style="width:210px" value=""></el-input>
<!--                    </el-select>-->
                  </el-form-item>

                  <el-form-item label="发放："  style="width:500px">
                    <el-input-number
                        v-model="num"
                        :min="1"
                        :max="10000"
                        controls-position="right"
                        @change="handleChange"
                        size="small"
                    />
                    <span>元 × 出差的小时数</span>
                  </el-form-item>


                  <el-form-item label="备注" prop="businessRemark" style="width:500px">
                    <el-input v-model="businessform.businessRemark" type="textarea"></el-input>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" style="width: 60px;" @click="submitForm('ruleForm')"
                    >提交</el-button
                    >
                    <router-link to="/evection">&nbsp;
                      <el-button style="width: 60px;" @click="goblack()">取消</el-button></router-link>
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
      businessform: {
        //出差方案名称
        businessName: '',
        //
        businesswage: '按出差时长',
        //备注
        businessRemark: '',
      },
      //存出差方案信息
      businessMsg:[],
      num: '150',
      rules: {
        schemename:[
          {
            required: true,
            message: '请输入方案名称',
            trigger: 'blur',
          }
        ],
        // businesswage: [
        //   {
        //     required: true,
        //     message: '请选择出差工资规则',
        //     trigger: 'change',
        //   },
        // ],
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
            this.insertBusiness();
          }else{
            this.updateBusiness()
          }

        } else {
          console.log('error submit!!')

        }
      })
    },
    //新增出差方案
    insertBusiness(){
      alert(this.businessform.businessName)
      this.axios({
        url: 'http://localhost:8010/provider/business/insertBusiness',
        method: 'post',
        data:{

          // 出差方案名称
          businessName:this.businessform.businessName,
          //出差一天金额
          businessOnemoney:this.num,
          //状态
          businessState:0,
          //备注
          businessRemark:this.businessform.businessRemark,
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
    //根据id查询出差方案信息
    selectBusinessId() {
      alert(this.$route.query.id)
      this.axios
          .get("http://localhost:8010/provider/business/selectBusinessId/" + this.$route.query.id)
          .then((response) => {
            console.log(response);
            this.businessMsg = response.data.data;

            this.businessform.businessName=this.businessMsg.businessName;
            this.num=this.businessMsg.businessOnemoney;
            this.businessform.businessRemark=this.businessMsg.businessRemark;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //修改出差信息
    updateBusiness(){
      this.axios({
        url: 'http://localhost:8010/provider/business/updateBusiness',
        method: 'put',
        data: {
          //出差方案id
          businessId:this.$route.query.id,         // 出差方案名称
          businessName:this.businessform.businessName,
          //出差一天金额
          businessOnemoney:this.num,
          //备注
          businessRemark:this.businessform.businessRemark,
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
      this.selectBusinessId()
    }
  }
}
</script>


<style scoped>
@import url(../../css/dimission.css);
@import url("../../css/navigation.css");
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