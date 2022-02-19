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

<!--          <div>-->
<!--            <el-radio  v-model="radio1" label="1" size="large">给职位设置薪酬组</el-radio>&nbsp;&nbsp;&nbsp;&nbsp;-->
<!--            <el-radio @click="compensationStaff=true,compensationPost=false" v-model="radio1" label="2" size="large">给员工设置薪酬组</el-radio>-->
<!--          </div>-->
<!--          -->
          <el-form
              ref="ruleFormRef"
              :model="compensationForm"
              :rules="rules"
              label-width="120px"
              class="demo-ruleForm"
              :size="formSize"
          >
            <el-form-item label="组名称：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationForm.compensationName" style="width:240px"></el-input>
            </el-form-item>

            <el-form-item label="适用员工：" prop="name" style="margin-top: 20px;">
            <div> <el-input @click="StaffBecome=true" v-model="compensationForm.name" style="width:240px"></el-input></div>
            </el-form-item>

<!--            <el-form-item label="适用员工：" prop="name" style="margin-top: 20px;">-->
<!--              <div class="name_tb" >-->
<!--                <span style="margin-left: 10px;"></span>-->
<!--                <span @click="become = true"><i  class="iconfont" style="position: absolute;left:210px; cursor:pointer;width:30px">&#xe629;</i></span>-->
<!--              </div>-->
<!--              -->
<!--            </el-form-item>-->

            <el-form-item label="适用部门：" style="margin-top: 20px;">
              <el-select v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()" style="width:240px">
                <el-option hidden ></el-option>
                <el-option
                    class="xxx"
                    v-for="item in dept"
                    :key="item.deptId"
                    :label="item.deptName"
                    :value="item.deptId"
                >
                </el-option>
                <el-tree :data="deptlists"
                         show-checkbox
                         :default-expand-all=true
                         :check-on-click-node=true
                         node-key="deptId"

                         :props="defaultProps" ref="tree" @check-change="handleCheckChange()" />
              </el-select>
            </el-form-item>


            <el-form-item label="适用职位：" style="margin-top: 20px;">
              <el-select  placeholder="请选择试用职位" multiple v-model="compensationForm.citysPost" style="width:240px">
                <el-option
                    v-for="item in positionAll"
                    :key="item.positionId"
                    :label="item.positionName"
                    :value="item.positionId" />

              </el-select>
            </el-form-item>

            <el-form-item label="备注：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationForm.compensationRemark" style="width:240px"></el-input>
            </el-form-item>

            <el-button style="width:80px;margin-top: 30px;margin-left: 120px" @click="become=false">取消</el-button>
            <el-button type="primary" style="width:80px" @click="compensationSalary(),become=false">提交</el-button>

          </el-form>

<!--          <el-form v-show="compensationStaff"-->
<!--              ref="ruleFormRef"-->
<!--              :model="compensationStaffForm"-->
<!--              :rules="rules"-->
<!--              label-width="120px"-->
<!--              class="demo-ruleForm"-->
<!--              :size="formSize"-->
<!--          >-->
<!--            <el-form-item label="组名称：" prop="name" style="margin-top: 20px;">-->
<!--              <el-input v-model="compensationStaffForm.name" style="width:240px"></el-input>-->
<!--            </el-form-item>-->

<!--           -->


<!--            <el-form-item label="备注：" prop="name" style="margin-top: 20px;">-->
<!--              <el-input v-model="compensationStaffForm.name" style="width:240px"></el-input>-->
<!--            </el-form-item>-->

<!--            <el-button style="width:80px;margin-top: 30px;margin-left: 120px">取消</el-button>-->
<!--            <el-button type="primary" style="width:80px">提交</el-button>-->

<!--          </el-form>-->


        </el-dialog>
      </div>

<!--      点击选择员工弹出框-->
      <div>
        <el-dialog
            v-model="StaffBecome"
            title="选择员工"
            width="50%"
            :close-on-click-modal="false">
        <el-table>
          <el-table-column
              prop="staffName"
              label="姓名"
              width="180">
          </el-table-column>
          <el-table-column
              prop="deptName"
              label="部门"
              width="180">
          </el-table-column>
          <el-table-column
              prop="positionName"
              label="职位">
          </el-table-column>
          </el-table>

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
import {ElMessage} from "element-plus";

// const activeName = ref('first')


export default {


  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value:'deptId'
    }
    return {

      res:"",
      // 选中值1
      res1:"",
      // 选中值2
      res2:"",
      // 部门  文本框的值
      dept:[],
      deptId:[],
      // 格式
      defaultProps,
      //存放部门信息
      deptlists: [],

      //存储部门名称
      deptNameAll:[],
      //存储部门职位名称
      positionAll:[],
      //存选中的部门
      citysDept:[],

      StaffBecome:false,

      //薪酬组
      compensationVal:null,
      //薪酬组部门职位
      compensationDeptPost:null,


      radio1 :'1',
      become:false,
      compensationPost:true,
      compensationStaff:false,
      //
      compensationForm:{
        //薪酬组名称
        compensationName:"",
        //部门
        dept:"",
        //存选中的职位
        citysPost:[],
        //备注
        compensationRemark:'',

      },
      compensationStaffForm:{
        name:"",
      }
    }
  },methods:{

    // 当文本框值发生变化时调用的方法
    onchange(){
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys(this.deptId, false)
    },

    // 点击文本框时调用的方法
    onclicks() {

      // 取当前选择器中的复选框选项id
      this.res1 = this.$refs.tree.getCheckedKeys()
    },

    //节点选中状态发生变化时调用的方法
    handleCheckChange(data, checked, indeterminate) {

      //获取所有选中的节点 start
      this.res = this.$refs.tree.getCheckedNodes()

      // 取当前选择器中的复选框选项id
      this.res2 = this.$refs.tree.getCheckedKeys()

      // 清空部门
      this.dept = []
      // 清空选中的部门
      this.deptId = []
      let x = 0
      for (let i = 0; i < this.res.length; i++) {

        for (let j = 0; j < this.res.length; j++) {
          // 如果父id 不等于 id 就加入到数据中
          if (this.res[i].deptPid != this.res[j].deptId) {
            //并且是最后一个
            if (j == this.res.length - 1 && x == 0) {
              // 加入数据
              this.dept.push(this.res[i])
              // 赋值到文本框
              this.deptId.push(this.res[i].deptId)
            }

          } else {
            x = 1
          }
        }
        x = 0
      }
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
    },
    //取薪酬组input文本里面的值
    compensationSalary(){

      //薪酬组数据
      this.compensationVal={
        //取薪酬组名称
        compensationName:this.compensationForm.compensationName,
        //备注
        compensationRemark:this.compensationForm.compensationRemark,
      }

      console.log( this.$refs.tree.getCheckedKeys() )
      console.log( this.compensationForm.citysPost )

      this.insertcompensation()

    },
    //添加薪酬组
    insertcompensation(){
      this.axios({
        url: 'http://localhost:8010/provider/compensation/insertcompensation',
        method: 'post',
        data:{
          Compensation:this.compensationVal,
          //取部门信息
          deptIds:this.$refs.tree.getCheckedKeys(),
          //取职位信息
          postIds:this.compensationForm.citysPost
        }
      }).then(response => {
        console.log(response);
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

  }
}
</script>

<style scoped>
@import url("../../css/navigation.css");
@import url("../../css/Salary.css");

.xxx{
  display: none;
}
.name_tb{
  width:240px;
  height:40px;
  border:1px solid silver;
  border-radius: 3px;
  background: white;
  position: relative;
}

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

