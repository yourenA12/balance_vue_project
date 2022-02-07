<!--薪酬组 -->
<template>

  <div style="width:100%;">
    <div style="width:95%;margin: auto">


      <!--  弹框  -->
      <div>
        <el-dialog
            v-model="become"
            title="新增薪酬组"
            width="30%"
            :close-on-click-modal="false">

          <div>
            <el-radio @click="compensationPost=true,compensationStaff=false" v-model="radio1" label="1" size="large">给职位设置薪酬组</el-radio>&nbsp;&nbsp;&nbsp;&nbsp;
            <el-radio @click="compensationStaff=true,compensationPost=false" v-model="radio1" label="2" size="large">给员工设置薪酬组</el-radio>
          </div>
<!--          -->
          <el-form v-show="compensationPost"
              ref="ruleFormRef"
              :model="compensationForm"
              :rules="rules"
              label-width="120px"
              class="demo-ruleForm"
              :size="formSize"
          >
            <el-form-item label="组名称：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationForm.name" style="width:240px"></el-input>
            </el-form-item>

<!--            <el-form-item label="试用部门：" prop="name" style="margin-top: 20px;">-->
<!--              <el-input v-model="compensationForm.name" style="width:240px"></el-input>-->
<!--            </el-form-item>-->

            <el-form-item label="试用部门：" style="margin-top: 20px;">
              <el-select  placeholder="请选择试用部门" multiple v-model="citysDept" style="width:240px">
                <el-option
                    v-for="item in deptNameAll"
                    :key="item.deptId"
                    :label="item.deptName"
                    :value="item.deptId" />

              </el-select>
            </el-form-item>

<!--            <el-form-item label="试用职位：" prop="name" style="margin-top: 20px;">-->
<!--              <el-input v-model="compensationForm.name" style="width:240px"></el-input>-->
<!--            </el-form-item>-->

            <el-form-item label="试用职位：" style="margin-top: 20px;">
              <el-select  placeholder="请选择试用职位" multiple v-model="citysPost" style="width:240px">
                <el-option
                    v-for="item in positionAll"
                    :key="item.positionId"
                    :label="item.positionName"
                    :value="item.positionId" />

              </el-select>
            </el-form-item>

            <el-form-item label="备注：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationForm.name" style="width:240px"></el-input>
            </el-form-item>

            <el-button style="width:80px;margin-top: 30px;margin-left: 120px">取消</el-button>
            <el-button type="primary" style="width:80px">提交</el-button>

          </el-form>

          <el-form v-show="compensationStaff"
              ref="ruleFormRef"
              :model="compensationStaffForm"
              :rules="rules"
              label-width="120px"
              class="demo-ruleForm"
              :size="formSize"
          >
            <el-form-item label="组名称：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationStaffForm.name" style="width:240px"></el-input>
            </el-form-item>

            <el-form-item label="试用员工：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationStaffForm.name" style="width:240px"></el-input>
            </el-form-item>


            <el-form-item label="备注：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationStaffForm.name" style="width:240px"></el-input>
            </el-form-item>

            <el-button style="width:80px;margin-top: 30px;margin-left: 120px">取消</el-button>
            <el-button type="primary" style="width:80px">提交</el-button>

          </el-form>


        </el-dialog>
      </div>

    <el-button @click="become=true,selectDeptName(),selectPositionName()" size="small" type="primary" plain style="width: 80px">
      <el-icon><i-plus/></el-icon>
      新增
    </el-button>

      <div style="margin-top:30px;">
        <el-table :data="tableData" style="width: 100%"
                  :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                  :cell-style="{textAlign: 'center'}">

          <el-table-column  prop="staffName" label="薪酬组" width="200"/>
          <el-table-column prop="staffBirthday" label="试用部门" width="220"/>
          <el-table-column prop="deptName" label="试用人员" width="220"/>
          <el-table-column prop="postName" label="职位" width="220"/>
          <el-table-column prop="staffPhone" label="备注" width="220"/>
          <el-table-column  label="操作" width="180">
            <template #default="scope">
              <el-button type="text" size="small" @click="empMsg(scope.row.staffId)"
              >编辑
              </el-button>
              <!--                <router-link :to="{path:this.leave,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
              &nbsp;
              <el-button @click="departure(scope.row)" type="text" size="small">删除</el-button>
              <!--                </router-link>-->
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
  &nbsp;
</template>

<script >

import { ref } from 'vue'

// const activeName = ref('first')


export default {


  data() {
    return {

      //存储部门名称
      deptNameAll:[],
      //存储部门职位名称
      positionAll:[],
      //存选中的部门
      citysDept:[],
      //存选中的职位
      citysPost:[],

      radio1 :'1',
      become:false,
      compensationPost:true,
      compensationStaff:false,
      //
      compensationForm:{
        name:"",
      },
      compensationStaffForm:{
        name:"",
      }
    }
  },methods:{
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
    aa(){
      alert(1)
    }
  }
}
</script>

<style scoped>
@import url("../../css/navigation.css");
@import url("../../css/Salary.css");

.mainContent {
  width: 155px;
  height: 195px;
  border: 1px solid #e8edf2;
  border-radius: 10px;
  text-align: center;
  display: block;
  float: left;
  margin-right: 78px;
}
</style>

