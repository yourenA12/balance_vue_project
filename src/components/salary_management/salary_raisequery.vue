<template>
  <!-- 调薪查询 -->
<!--  <div class="saas-main-content">-->
<!--    <div class="j-card j-card-bordered mainContent">-->
<!--      <div class="j-card-body">-->
<!--        <el-tab-pane name="second">-->
<!--          <template #label>-->
<!--            <span @click="disly=false,disly_1=false">调薪查询</span>-->
<!--          </template>-->
<!--          <el-select v-model="dept2" placeholder="请选择">-->
<!--            <el-option label="什么什么部门" value="dept1"></el-option>-->
<!--            <el-option label="上山不" value="dept2"></el-option>-->
<!--          </el-select>-->
          <div style="width: 100%;height:80px;margin-top: 20px">
            <div>
              <!--搜索输入框-->
              <el-row style="width:200px;float: right;margin-left: 30px">
                <el-input v-model="input3" placeholder="搜索">
                  <template #suffix>
                    <el-icon class="el-input__icon">
                      <i-search/>
                    </el-icon>
                  </template>
                </el-input>
              </el-row>
            </div>

            <div style="float: right;">
              <span class="demonstration"></span>
              <el-date-picker
                  v-model="seek2"
                  type="datetimerange"
                  range-separator="-"
                  start-placeholder="开始时间"
                  end-placeholder="结束时间"
              >
              </el-date-picker>
            </div>


          </div>

          <el-table :data="tableData" style="width: 100%"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column fixed prop="date" label="姓名" width="150"/>
            <el-table-column prop="city" label="部门" width="150"/>
            <el-table-column prop="name" label="调薪前工资" width="150"/>
            <el-table-column prop="state" label="调薪后工资" width="150"/>
            <el-table-column prop="address" label="调薪生效日期" width="150"/>
            <el-table-column prop="zip" label="调整原因" width="150"/>
            <el-table-column prop="zip" label="备注" width="150"/>
            <el-table-column prop="zip" label="操作人" width="150"/>
            <el-table-column prop="zip" label="操作日期" width="150"/>
            <el-table-column prop="zip" label="状态" width="150"/>


          </el-table>
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
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      tableData: [{
        date: '2016-05-03',
        name: 'Tom',
        state: 'California',
        city: 'Los Angeles',
        address: '11111111',
        zip: 'CA 90036',
        tag: 'Home',
      },
        {
          date: '2016-05-02',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: '11111111',
          zip: 'CA 90036',
          tag: 'Office',
        },
        {
          date: '2016-05-04',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: '11111111',
          zip: 'CA 90036',
          tag: 'Home',
        },
        {
          date: '2016-05-01',
          name: 'Tom',
          state: 'California',
          city: 'Los Angeles',
          address: '11111111',
          zip: 'CA 90036',
          tag: 'Office',
        },
      ]
    }
  },
  methods: {
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
    }
  }
}
</script>

<style>
 .demo-pagination-block{
   float: right;
   margin-top:10px;
 }
</style>