<!--薪酬统计 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">

      <div class="j-card-body">
        <div class="j-card-head">
          <div class="j-card-head-title" style="margin-left:20px;">
            <span>薪酬统计</span>
          </div>
        </div>
        <br/>

        <div class="container">
          <div class="block" style="margin-left: 1095px">
            <span class="demonstration"></span>
            <el-date-picker v-model="months" type="year" placeholder="请选择年份">
            </el-date-picker>
          </div>
        </div>

        <br/>
        <div class="demo-collapse">
          <el-collapse @change="handleChange" accordion>
            <div v-for="(item,index) in salaryStatistics">
              <el-collapse-item :name="item.month">
                <template #title>
                  <span style="margin-left: 30px;font-weight: bolder">{{item.month}}月份</span>
                  <span style="margin-left: 160px">计薪人数：{{ item.askperson }}</span>
                  <span style="margin-left: 160px">应发工资：{{ item.salary }}</span>
                  <span style="margin-left: 160px">实发工资：{{ item.payrollsalary }}</span>
                  <span style="margin-left: 160px">公司缴纳：{{ item.firmpayment }}</span>
                </template>

                <div style="background-color:  #e8edf2">
                  <el-table
                      :data="tableData"
                      style="width: 1275px; margin: auto"
                      :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                      :cell-style="{textAlign: 'center'}">
                    <el-table-column fixed prop="compensationName" label="薪酬组" />
                    <el-table-column prop="wagenotfiledAskperson" label="计薪人数" />
                    <el-table-column prop="wagenotfiledSalary" label="应发工资" />
                    <el-table-column prop="wagenotfiledPayrollsalary" label="实发工资" />
                    <el-table-column prop="wagenotfiledFirmpayment" label="公司缴纳" />
<!--                    <el-table-column prop="zip" label="考勤扣款" width="120"/>-->
<!--                    <el-table-column prop="state" label="应发工资" width="120"/>-->
<!--                    <el-table-column prop="city" label="个人缴社保" width="120"/>-->
<!--                    <el-table-column prop="address" label="个人缴公积金" width="120"/>-->
<!--                    <el-table-column prop="zip" label="实发工资" width="120"/>-->
<!--                    <el-table-column prop="state" label="公司缴社保" width="120"/>-->
<!--                    <el-table-column prop="city" label="公司缴公积金" width="120"/>-->
<!--                    <el-table-column prop="address" label="公司缴纳" width="120"/>-->

                  </el-table>

                </div>

              </el-collapse-item>
              <br>
            </div>

          </el-collapse>
          <br/>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      tableData: [],//表格数据
      months: '',
      salaryStatistics:[],// 薪酬统计

    }
  },
  methods: {

    // 点击折叠面板发生变化时调用的方法
    handleChange(val) {
      // 按月份查询已归档数据
      this.selectWagenotfiledVo(val)
    },

    // 所有月份的薪酬统计
    selectSalaryStatistics(){
      this.axios
          .get("http://localhost:8010/provider/salaryStatistics/salaryStatistics",)
          .then((response) => {
            console.error(response);
            this.salaryStatistics=response.data.data
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 按月份查询已归档数据
    selectWagenotfiledVo(date){
      this.axios
          .get("http://localhost:8010/provider/salaryStatistics/selectWagenotfiledVo/"+date)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data;
          })
          .catch(function (error) {
            console.log(error);
          });
    },

  },
  created() {
    // 所有月份的薪酬统计
    this.selectSalaryStatistics()
  }
}
</script>


<style scoped>
@import url("../../css/navigation.css");
@import url(../../css/dimission.css);

/deep/ .demo-collapse {
  margin-left: 10PX;
}

/deep/ .el-collapse-item__header {
  display: flex;
  align-items: center;
  height: var(--el-collapse-header-height);
  line-height: var(--el-collapse-header-height);
  cursor: pointer;
  border-bottom: 1px solid var(--el-collapse-border-color);
  font-size: var(--el-collapse-header-font-size);
  font-weight: 500;
  transition: border-bottom-color var(--el-transition-duration);
  outline: 0;
  background-color: #e8edf2;
}
</style>
