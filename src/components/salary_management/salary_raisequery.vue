<template>
  <!-- 调薪查询 -->

  <div style="margin-top:30px;margin-left: 850px">
    <!--搜索输入框-->
    <div style="display: inline-block">
      <!--                  <span style="font-weight:bold;margin-left: 30px">员工名称</span>-->
      <el-row style="width:200px;display: inline-block;margin-left: 15px">
        <el-input v-model="pageInfo.staffName" placeholder="请输入员工名称"/>
      </el-row>
    </div>


    <el-button @click="selectSalary()" type="primary" style="width: 80px;margin-left:25px">
      <el-icon>
        <i-search/>
      </el-icon>
      搜索
    </el-button>
    <el-button style="width: 80px;" @click="replacements()">
      <el-icon>
        <i-refresh/>
      </el-icon>
      重置
    </el-button>
  </div><br/>

          <el-table :data="salaryData" style="width: 100%"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column fixed prop="staffName" label="姓名" width="150"/>
            <el-table-column prop="deptName" label="部门" width="150"/>
            <el-table-column prop="frontSalary" label="调薪前工资" width="150"/>
            <el-table-column prop="afterSalary" label="调薪后工资" width="150"/>
            <el-table-column prop="takeEffectDate" label="调薪生效日期" width="150"/>
            <el-table-column prop="salaryCause" label="调整原因" width="150"/>
            <el-table-column prop="salaryRemarks" label="备注" width="150"/>
<!--            <el-table-column prop="zip" label="操作人" width="150"/>-->
            <el-table-column prop="createdTime" label="操作日期" width="150"/>
            <el-table-column prop="salaryState" label="状态" width="150">
              <template #default="scope">
                <span v-if="scope.row.salaryState==0" style="color: red">不同意</span>
                <span v-if="scope.row.salaryState==1" style="color:#6ba1e0;">同意</span>
              </template>
            </el-table-column>


          </el-table>
          <!-- 分页插件 -->
          <div class="demo-pagination-block">
            <el-pagination
                v-model:currentPage="pageInfo.currenPage"
                :page-sizes="[3, 5, 10, 50]"
                v-model:page-size="pageInfo.pagesize"
                :default-page-size="pageInfo.pagesize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="pageInfo.total"
                :pager-count="5"
                background
            >
              <!--  @size-change="selectUsers" @current-change="selectUsers" -->
            </el-pagination>
          </div>
<!--        </el-tab-pane>-->
<!--      </div>-->
<!--    </div>-->
<!--  </div>-->

</template>

<script>
import {
  defineComponent,
  ref
} from 'vue'
import {ElMessage} from "element-plus";


export default {
  data() {
    return {
      disly: false,
      disly_1: false,
      activeName: 'first',
      periodpay: '',
      duepay: '',
      periodfixed: '',
      duefixed: '',
      postdate: '',
      remark: '',
      increasepay: '',
      afterpay: '',
      takedate: '',
      cause: '',
      remark2: '',
      seek: '',
      dept2: '',
      seek2: '',
      pageInfo: {
        // 分页参数
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        //员工名称
        staffName:'',
      },
      //存查询调薪记录信息
      salaryData: []
    }
  },
  methods: {
    //搜索框重置
    replacements() {
      this.pageInfo.currenPage = 1
      this.pageInfo.staffName = ''

      this.selectSalary()

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
        alert(1111)
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
    //查询调薪记录信息
    selectSalary() {

      this.axios
          .get("http://localhost:8010/provider/salary/selectSalary",{params:this.pageInfo})
          .then((response) => {
            console.log(response);
            this.salaryData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectSalary()
  }
}
</script>

<style>
 .demo-pagination-block{
   float: right;
   margin-top:10px;
 }
</style>