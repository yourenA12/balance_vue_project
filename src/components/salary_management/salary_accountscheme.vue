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
              <div>
                <el-select ref="vueSelects" @click="staffBecome=true,staffSelect()" v-model="compensationForm.name"
                           placeholder="请选择适用员工" multiple style="width:240px">
                  <el-option
                      class="xxx"
                      v-for="item in tableVal"
                      :key="item.staffId"
                      :label="item.staffName"
                      :value="item.staffId"/>
                </el-select>

              </div>
            </el-form-item>


            <el-form-item label="适用部门：" style="margin-top: 20px;">
              <el-select v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()"
                         style="width:240px">
                <el-option hidden></el-option>
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

                         :props="defaultProps" ref="tree" @check-change="handleCheckChange()"/>
              </el-select>
            </el-form-item>


            <el-form-item label="适用职位：" style="margin-top: 20px;">
              <el-select placeholder="请选择试用职位" multiple v-model="compensationForm.citysPost" style="width:240px">
                <el-option
                    v-for="item in positionAll"
                    :key="item.positionId"
                    :label="item.positionName"
                    :value="item.positionId"/>

              </el-select>
            </el-form-item>

            <el-form-item label="备注：" prop="name" style="margin-top: 20px;">
              <el-input v-model="compensationForm.compensationRemark" style="width:240px"></el-input>
            </el-form-item>

            <el-button style="width:80px;margin-top: 30px;margin-left: 120px" @click="become=false,replacement()">取消</el-button>
            <el-button type="primary" style="width:80px" @click="compensationSalary()">提交</el-button>

          </el-form>


        </el-dialog>
      </div>

      <!--      点击选择员工弹出框-->
      <div>
        <el-dialog
            v-model="staffBecome"
            title="选择员工"
            width="50%">

          <div style="display: inline-block;margin-left: 290px">
            <span style="font-weight:bold">部门 </span>

            <el-select v-model="deptIdy" multiple ref="vueSelect" @change="onchangey()" @click="onclicksy()">
              <el-option hidden></el-option>
              <el-option
                  class="xxx"
                  v-for="item in depty"
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
                       :props="defaultProps" ref="treey" @check-change="handleCheckChangey()"/>
            </el-select>
          </div>
          <el-button @click="selectStaffXX()" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button @click="replacement1()" style="width: 80px;">
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>


          <el-table
              :data="deptData"
              @selection-change="staffAll"
              height="250"
              style="width: 100%;margin-top: 20px;"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}">

            <!-- 全选操作按钮 -->
            <el-table-column type="selection" width="90"/>

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

          <div class="demo-pagination-block">
            <el-pagination
                v-model:currentPage="pageInfo1.currenPage"
                :page-sizes="[3, 5, 10, 50]"
                v-model:page-size="pageInfo1.pagesize"
                :default-page-size="pageInfo1.pagesize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="pageInfo1.total"
                :pager-count="5"
                background
                @size-change="selectStaffXX()"
                @current-change="selectStaffXX()"
            >
            </el-pagination>
          </div>

          <div style="margin-top: 60px;margin-left:280px">
          </div>

        </el-dialog>
      </div>

      <el-button @click="become=true,selectDeptName(),selectPositionName()" size="small" type="primary" plain
                 style="width: 80px">
        <el-icon>
          <i-plus/>
        </el-icon>
        新增
      </el-button>

      <div style="margin-top:30px;">
        <el-table :data="tableData" style="width: 100%"
                  :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                  :cell-style="{textAlign: 'center'}">

          <el-table-column prop="staffName" label="薪酬组" width="200"/>
          <el-table-column prop="staffBirthday" label="试用部门" width="220"/>
          <el-table-column prop="deptName" label="试用人员" width="220"/>
          <el-table-column prop="postName" label="职位" width="220"/>
          <el-table-column prop="staffPhone" label="备注" width="220"/>
          <el-table-column label="操作" width="180">
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

<script>

import {ref} from 'vue'
import {ElMessage} from "element-plus";
import qs from "qs";

// const activeName = ref('first')


export default {


  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value: 'deptId'
    }
    return {

      res: "",
      // 选中值1
      res1: "",
      // 选中值2
      res2: "",
      // 部门  文本框的值
      dept: [],
      deptId: [],
      // 格式
      defaultProps,
      //存放部门信息
      deptlists: [],


      resy: "",
      // 选中值1
      res1y: "",
      // 选中值2
      res2y: "",
      // 部门  文本框的值
      depty: [],
      deptIdy: [],

      //弹出框(员工)的分页
      pageInfo1: {
        // 分页参数
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        //部门
        deptSearch: '',
      },
      //弹出框(员工)存储数据
      deptData: [],
      // 单选框选择员工
      radioStaff: "",

      //存储部门名称
      deptNameAll: [],
      //存储部门职位名称
      positionAll: [],
      //存选中的部门
      citysDept: [],

      // 员工多选时的值
      tableVal: "",

      staffBecome: false,

      CompensationNameAll: [],

      //薪酬组
      compensationVal: null,
      //薪酬组部门职位
      compensationDeptPost: null,


      radio1: '1',
      become: false,
      compensationPost: true,
      compensationStaff: false,
      //
      compensationForm: {
        //薪酬组名称
        compensationName: "",
        //员工名称
        name:'',
        //部门
        dept: "",
        //存选中的职位
        citysPost: [],
        //备注
        compensationRemark: '',

      },

    }
  }, methods: {

    //弹出框重置
    replacement() {
     this.compensationForm.compensationName=''
      this.compensationForm.compensationRemark=''
      this.compensationForm.name='',
      this.compensationForm.citysPost = ''
      // this.res2=""
      // // 将值赋值到选择器中
      // this.$refs.tree.setCheckedKeys([], false)




    },

    //搜索框重置
    replacement1() {
      this.res2y = ""
      // 将值赋值到选择器中
      this.$refs.treey.setCheckedKeys([], false)

      // 取消单选框选中状态
      this.radioStaff = ""

      this.selectStaffXX()

    },


    // 选中员工的值
    // 选中值发生变化时调用的方法
    staffAll(val) {
      // 选中的值
      this.tableVal = val
      // 清空选中 id
      this.compensationForm.name = []

      // 循环获取选中行的id
      for (let i = 0; i < val.length; i++) {
        this.compensationForm.name.push(val[i].staffId)
      }
    },

// 点击时 关闭 下拉框
    staffSelect() {
      // 关闭选择器
      this.$refs.vueSelects.blur();
    },

    // 当文本框值发生变化时调用的方法
    onchange() {
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


    // 当文本框值发生变化时调用的方法
    onchangey() {
      // 将值赋值到选择器中
      this.$refs.treey.setCheckedKeys(this.deptIdy, false)
    },

    // 点击文本框时调用的方法
    onclicksy() {

      // 取当前选择器中的复选框选项id
      this.res1y = this.$refs.treey.getCheckedKeys()
    },

    //节点选中状态发生变化时调用的方法
    handleCheckChangey(data, checked, indeterminate) {

      //获取所有选中的节点 start
      this.resy = this.$refs.treey.getCheckedNodes()

      // 取当前选择器中的复选框选项id
      this.res2y = this.$refs.treey.getCheckedKeys()

      // 清空部门
      this.depty = []
      // 清空选中的部门
      this.deptIdy = []
      let x = 0
      for (let i = 0; i < this.resy.length; i++) {

        for (let j = 0; j < this.resy.length; j++) {
          // 如果父id 不等于 id 就加入到数据中
          if (this.resy[i].deptPid != this.resy[j].deptId) {
            //并且是最后一个
            if (j == this.resy.length - 1 && x == 0) {
              // 加入数据
              this.depty.push(this.resy[i])
              // 赋值到文本框
              this.deptIdy.push(this.resy[i].deptId)
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
    //查询员工的信息 姓名 部门 职位
    selectStaffXX() {

      let params = {

        currenPage: this.pageInfo1.currenPage,
        pagesize: this.pageInfo1.pagesize,
        deptIds: this.res2y.length == 0 ? '' : this.res2y,

      }
      this.axios
          .get("http://localhost:8010/provider/staff/selectStaffXX?" + qs.stringify(params, {arrayFormat: 'repeat'}))
          .then((response) => {
            console.log(response);
            this.deptData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo1.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });

    },
    //取薪酬组input文本里面的值
    compensationSalary() {

      //薪酬组数据
      this.compensationVal = {
        //取薪酬组名称
        compensationName: this.compensationForm.compensationName,
        //备注
        compensationRemark: this.compensationForm.compensationRemark,
      }

      console.log(this.$refs.tree.getCheckedKeys())
      console.log(this.compensationForm.citysPost)

      this.insertcompensation()

    },
    //添加薪酬组
    insertcompensation() {
      this.axios({
        url: 'http://localhost:8010/provider/compensation/insertcompensation',
        method: 'post',
        data: {
          Compensation: this.compensationVal,
          //取部门信息
          deptIds: this.res2,
          //取职位信息
          postIds: this.compensationForm.citysPost,
          //获取员工id
          staffIds :this.compensationForm.name
        }
      }).then(response => {
        console.log(response);
        if (response.data.data == "添加成功") {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.become=false
          this.replacement() // 添加完情况文本框

        } else if(response.data.data == "添加失败"){
          ElMessage.error('添加失败')
        }else{
          ElMessage.warning(response.data.data)
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
    //根据薪酬组名称查询数据
    // selectCompensationNames() {
    //
    //   this.axios({
    //     url: 'http://localhost:8010/provider/compensation/insertcompensation',
    //     method: 'post',
    //     data: {
    //       Compensation: this.compensationVal,
    //       //取部门信息
    //       deptIds: this.$refs.tree.getCheckedKeys(),
    //       //取职位信息
    //       postIds: this.compensationForm.citysPost,
    //       //获取员工id
    //       staffId :this.compensationForm.name
    //     }
    //   }).then(response => {
    //     console.log(response);
    //     if (response.data.data > 0) {
    //       ElMessage({
    //         message: '添加成功',
    //         type: 'success',
    //       })
    //       this.selectEmps() // 修改完成后调用查询方法
    //     } else {
    //       ElMessage.error('添加失败')
    //     }
    //   }).catch(function (error) {
    //     console.log(error);
    //   });
    //
    // },

  }, created() {
    this.selectStaffXX()
    // this.selectCompensationNames()
  }
}
</script>

<style scoped>
@import url("../../css/navigation.css");
@import url("../../css/Salary.css");

.xxx {
  display: none;
}

.name_tb {
  width: 240px;
  height: 40px;
  border: 1px solid silver;
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

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>

