<!--固定方案 -->
<template>


        <div class="body_2">
          <el-tabs v-model="activeName" @tab-click="handleClick">


            <!-- body_3 隐藏的那个框 -->
<!--            <div class="body_3" v-show="disly">-->
<!--              <h3 style="margin-top:1px;">定薪</h3><br/>-->
<!--              <el-form :inline="true" ref="formInline" :model="formInline" :rules="rules" class="demo-form-inline">-->
<!--                <el-form-item>-->
<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">试用期基本工资</div>-->
<!--                  </template>-->
<!--                  <el-input v-model="periodpay" placeholder="请输入"/>-->
<!--                </el-form-item>-->
<!--                <el-form-item>-->
<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">正式基本工资</div>-->
<!--                  </template>-->
<!--                  <el-input v-model="duepay" placeholder="请输入"/>-->
<!--                </el-form-item>-->
<!--                <br>-->
<!--                <el-form-item>-->
<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">试用期固定工资</div>-->
<!--                  </template>-->
<!--                  <el-input v-model="periodfixed" placeholder="请输入"/>-->
<!--                </el-form-item>-->
<!--                <el-form-item>-->
<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">正式固定工资</div>-->
<!--                  </template>-->
<!--                  <el-input v-model="duefixed" placeholder="请输入"/>-->
<!--                </el-form-item>-->
<!--                <br>-->
<!--                &lt;!&ndash; 转正日期输入框 &ndash;&gt;-->
<!--                <el-form-item>-->

<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">转正日期</div>-->
<!--                  </template>-->
<!--                  <div class="block">-->
<!--                    <el-date-picker style="width: 210px;" v-model="postdate" type="date" placeholder="请选择" disabled>-->

<!--                    </el-date-picker>-->
<!--                  </div>-->
<!--                </el-form-item>-->
<!--                <el-form-item>-->
<!--                  <template #label>-->
<!--                    <div class="el-form-item__label">备注</div>-->
<!--                  </template>-->
<!--                  <el-input v-model="remark" placeholder="请输入"/>-->
<!--                </el-form-item>-->

<!--                <br>-->
<!--                &lt;!&ndash; 按钮 &ndash;&gt;-->
<!--                <div style="margin-top: 30px;">-->
<!--                  <el-button @click="disly=!disly,RestForm()">取消</el-button>-->
<!--                  <el-button type="primary" @click="submitForm('ruleForm')">保存</el-button>-->
<!--                </div>-->
<!--              </el-form>-->
<!--            </div>-->


            <!--  第二个隐藏框      -->
            <div class="body_3" v-show="disly_1">
              <h3 style="margin-top:1px;">调薪</h3><br/>
              <el-form :inline="true" :model="formInline" :rules="rules" class="demo-form-inline">
                <el-form-item>
                  <template #label>
                    <div class="el-form-item__label">调薪前基本工资</div>
                  </template>
                  <el-input v-model="afterpay" disabled/>
                </el-form-item>
                <el-form-item>
                  <template #label>
                    <div class="el-form-item__label">调薪后基本工资</div>
                  </template>
                  <el-input v-model="increasepay"/>
                </el-form-item>
                <br>
                <el-form-item>
                  <template #label>
                    <div class="el-form-item__label">生效日期</div>
                  </template>
                  <div class="block">
                    <el-date-picker style="width: 210px;" v-model="takedate" type="date" @change="takedates()"
                                    placeholder="请选择">

                    </el-date-picker>
                  </div>
                </el-form-item>
                <el-form-item label="调薪原因">
                  <el-select v-model="cause" placeholder="请选择">
                    <el-option label="晋升" value="晋升"></el-option>
                    <el-option label="调岗" value="调岗"></el-option>
                    <el-option label="降级" value="降级"></el-option>
                    <el-option label="其他" value="其他"></el-option>
                  </el-select>
                </el-form-item>

                <div style="margin-left: -430px;">
                  <el-form-item>
                    <template #label>
                      <div class="el-form-item__label">备注</div>
                    </template>
                    <el-input v-model="remark2" placeholder="请输入"/>
                  </el-form-item>
                </div>
                <br>
                <br>
                <!-- 按钮 -->
                <div style="margin-top: 30px;">
                  <el-button @click="disly_1=!disly_1,RestForm()">取消</el-button>
                  <el-button type="primary" @click="submitForm2('ruleForm')">保存</el-button>
                </div>
              </el-form>
            </div>


<!--            <el-tab-pane label="固定工资" name="first">-->
              <!-- 测试下拉按钮 -->

            <div style="margin-top:10px;">
              <!--搜索输入框-->
              <div style="display: inline-block">
                <span style="font-weight:bold;margin-left: 30px">员工名称</span>
                <el-row style="width:200px;display: inline-block;margin-left: 15px">
                  <el-input v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>
                </el-row>
              </div>

              <div style="display: inline-block;margin-left:25px">
                <span style="font-weight:bold;">部门 </span>

                <el-select v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()" >
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
                           :props="defaultProps" ref="tree" @check-change="handleCheckChange()" />
                </el-select>

              </div>

              <div style="display: inline-block;margin-left:25px">
                <span style="font-weight:bold">职位 </span>

                <el-select v-model="pageInfo.postSearch" placeholder="请输入职位名称" style="width: 200px;margin-left: 15px">
                  <el-option
                      v-for="item in positionAll"
                      :key="item.positionId"
                      :label="item.positionName"
                      :value="item.positionId"
                  >
                  </el-option>
                </el-select>

              </div>


              <el-button @click="selectFixedsalary()" type="primary" style="width: 80px;margin-left:25px">
                <el-icon>
                  <i-search/>
                </el-icon>
                搜索
              </el-button>
              <el-button style="width: 80px;" @click="replacement()">
                <el-icon>
                  <i-refresh/>
                </el-icon>
                重置
              </el-button>
            </div><br/>
            <div >
              <el-table :data="fixedData" style="width: 100%;"
                        :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                        :cell-style="{textAlign: 'center'}">
                <el-table-column fixed prop="staffName" label="姓名" width="180"/>
                <el-table-column prop="deptName" label="部门" width="180"/>
                <el-table-column prop="positionName" label="职位" width="180"/>
                <el-table-column prop="staffHiredate" label="入职日期" width="180"/>
                <el-table-column prop="staffState" label="状态" width="200">
                  <template #default="scope">
                    <span style="color:#fa8c16;" v-if="scope.row.staffState==2">试用</span>
                    <span style="color: #13c2c2;" v-if="scope.row.staffState==3">正式</span>

                  </template>
                </el-table-column>
                <el-table-column prop="zip" label="基本工资" width="200">
                  <template #default="scope">
                    <span v-if="scope.row.staffState==2" >{{scope.row.fixedwagePeriodmoney}}</span>
                    <span v-if="scope.row.staffState==3" >{{scope.row.fixedwageOfficialmoney}}</span>

                  </template>
                </el-table-column>
                <el-table-column fixed="right" label="操作" width="180">
                  <template #default="scope">
<!--                    <el-button type="text" size="small" @click="disly=true,disly_1=false">定薪</el-button>-->
                    <el-button type="text" size="small" v-if="scope.row.staffState==3" @click="disly_1=!disly_1,adjustment(scope.row)">调薪</el-button>

                  </template>
                </el-table-column>
              </el-table>
            </div>
              <!-- 分页插件 -->
              <div class="demo-pagination-block">
                <el-pagination
                    v-model:currentPage="pageInfo.currentPage"
                    :page-sizes="[3, 5, 10, 50]"
                    v-model:page-size="pageInfo.pagesize"
                    :default-page-size="pageInfo.pagesize"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="pageInfo.total"
                    :pager-count="5"
                    background
                    @size-change="selectFixedsalary"
                    @current-change="selectFixedsalary"
                >
                  <!--  @size-change="selectUsers" @current-change="selectUsers" -->
                </el-pagination>
              </div>
          </el-tabs>
        </div>




</template>

<script>
import {
  defineComponent,
  ref
} from 'vue'
import {ElMessage} from "element-plus";
import qs from "qs";

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



      //存储职位名称
      positionAll:[],
      // disly: false,
      disly_1: false,
      activeName: 'first',

      // 固定工资编号
      fixedwageId:"",
      //员工id
      staffId:'',
      //调薪后基本工资
      increasepay: '',
      //调薪前基本工资
      afterpay: '',
      //生效日期
      takedate: new Date(),
      //调薪原因
      cause: '',
      //备注
      remark2: '',
      seek: '',
      dept2: '',
      seek2: '',
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',
        //部门职位名称
        postSearch:'',
      },

      salaryVal:null,
      fixedwageVal:null,

      fixedData: []
    }
  },
  methods: {
    //搜索框重置
    replacement() {
      this.pageInfo.currentPage = 1,
          this.pageInfo.staffNameSearch = '',
          this.res2=""
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys([], false)
      this.pageInfo.postSearch = '',

          this.selectFixedsalary()

    },


    handleClick(tab, event) {
      console.log(tab, event)

    },
    submitForm() {
      if (this.duepay.length == 0) {
        ElMessage({
          message: '请输入正式基本工资',
          type: 'warning',
        })
      } else if (this.duefixed.length == 0) {
        ElMessage({
          message: '请输入正式固定工资',
          type: 'warning',
        })
      } else {
        alert(1111)
      }
    },
    submitForm2() {
      if (this.increasepay.length == 0) {
        ElMessage({
          message: '请输入调薪后基本工资',
          type: 'warning',
        })
      } else if (this.takedate.length == 0) {
        ElMessage({
          message: '请输入生效日期',
          type: 'warning',
        })
      } else if (this.cause.length == 0) {
        ElMessage({
          message: '请输入调薪原因',
          type: 'warning',
        })
      } else {
        this.insertFixedsalary_Val()
      }
    },
    RestForm() {
      this.periodpay = '',
          this.duepay = '',
          this.periodfixed = '',
          this.duefixed = '',
          this.postdate = '',
          this.remark = '',
          this.increasepay = '',
          this.range = '',
          this.takedate = '',
          this.cause = '',
          this.remark2 = ''
    },
    takedates() {
      var date = new Date();
      if (this.takedate < date) {
        ElMessage({
          message: '生效日期不能小于当前日期',
          type: 'warning',
        })
      }
    },
    //查询固定工资信息
    selectFixedsalary() {

      let params= {
        compId:this.$route.query.compId,
        currentPage:this.pageInfo.currentPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,
        postSearch:this.pageInfo.postSearch,
      }
      this.axios
          .get("http://localhost:8010/provider/fixedwage/selectFixedwage?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(response);
            this.fixedData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //点击调薪将表格中数据赋值到input文本中
    adjustment(row){

          //调薪前基本工资
      if(row.staffState==2){
        this.afterpay=row.fixedwagePeriodmoney
      }else if(row.staffState==3){
        this.afterpay=row.fixedwageOfficialmoney
      }
      //员工编号、
      this.staffId=row.staffId

      // 固定工资编号
      this.fixedwageId=row.fixedwageId
    },

    insertFixedsalary_Val(){
      //获取值,加入到调薪表
      this.salaryVal={
        //员工编号
        staffId:this.staffId,
        //调薪原因
        salaryCause:this.cause,
        //调薪前基本工资
        frontSalary:this.afterpay,
        //调薪后基本工资
        afterSalary:this.increasepay,
        //生效日期
        takeEffectDate:this.takedate,
        //备注
        salaryRemarks:this.remark2,
        //状态
        salaryState:1
      }
      //修改固定工资表
      this.fixedwageVal={
        //
        fixedwageId:this.fixedwageId,
        fixedwageOfficialmoney:this.increasepay
      }
      this.insertFixedsalary()
    },

    //添加调薪表
    insertFixedsalary(){

      this.axios({
        url: 'http://localhost:8010/provider/salary/insertSalary',
        method: 'post',
        data:{
          Salary:this.salaryVal,
          Fixedwage:this.fixedwageVal
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.disly_1=false
          this.selectFixedsalary()
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },
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


  },created() {
    this.selectFixedsalary()
    this.selectPositionName()
    this.selectDeptName()
  }
}
</script>

<style scoped>
#tab-first {
  margin: 40px
}

/deep/ .body_1 {
  background: #fff;
  border-radius: 4px;
  /* font-size: 14px; */
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  min-height: 100%;
  padding: 16px;
  box-sizing: border-box;
  border: 1px solid #e9e9e9;
  position: relative;
  padding: 0 24px;
  color: rgba(0, 0, 0, 0.65);
}
.xxx{
  display: none;
}

/deep/ .body_2 {
  background: #fff;
  border-radius: 4px;
  /* font-size: 14px; */
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  min-height: 100%;
  min-height: 720px;
  padding: 16px;
  box-sizing: border-box;
  border: 1px solid #e9e9e9;
  position: relative;
  padding: 0 24px;
  color: rgba(0, 0, 0, 0.65);
  margin: 20px;

}

/deep/ .body_3 {
  background: #e8edf2;
  padding: 24px 0 34px 24px;
  min-height: 100%;
  box-sizing: border-box;
  /* display: none; */
  text-align: center;
  border: 1px solid #e9e9e9;
}

/deep/ .body_4 {
  background: #e8edf2;
  padding: 24px 0 34px 24px;
  min-height: 100%;
  box-sizing: border-box;
  /* display: none; */

  text-align: center;
  border: 1px solid #e9e9e9;
}

/deep/ .el-form--inline .el-form-item {
  margin-right: 110px;
}

/deep/ .el-form-item__label {
  width: 110px;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>
