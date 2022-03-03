<!--查看工资表 -->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">

            <!--搜索输入框-->
          <div style="width:95%;height:50px;margin: auto;">
            <el-row style="width:200px;float: right;margin-top: 30px;">
              <el-input v-model="seek" placeholder="搜索">
                <template #suffix>
                  <el-icon class="el-input__icon">
                    <i-search/>
                  </el-icon>
                </template>
              </el-input>
            </el-row>
          </div>
          <div style="width:100%;margin-top: 50px;" class="icon-p">
            <div style="width:95%;margin: auto;">

              <div class="staff_div_5">

                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe614;</i>
                <div class="staff_div_5_text">
                  <span>全部</span><br/><span>2</span>
                </div>
              </div>

              <div class="staff_div_5 div_5_margin">
                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe60b;</i>
                <div class="staff_div_5_text">
                  <span>正式员工</span><br/><span>2</span>
                </div>
              </div>

              <div class="staff_div_5 div_5_margin">
                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe60c;</i>
                <div class="staff_div_5_text">
                  <span>实习员工</span><br/><span>2</span>
                </div>
              </div>

              <div class="staff_div_5 div_5_margin">
                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe61f;</i>
                <div class="staff_div_5_text">
                  <span>本月新入职</span><br/><span>2</span>
                </div>
              </div>

              <div class="staff_div_5 div_5_margin">
                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe616;</i>
                <div class="staff_div_5_text">
                  <span>本月离职</span><br/><span>2</span>
                </div>
              </div>
              <div class="staff_div_5 div_5_margin">
                <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe616;</i>
                <div class="staff_div_5_text">
                  <span>本月转正</span><br/><span>2</span>
                </div>
              </div>
            </div>

            <div style="width:95%;height:100px;margin: auto;background: #42b983;">
              <div style="text-align: center;">
                <el-table :data="tableData"
                          :header-cell-style="{textAlign: 'center',background:'#f8f8f9'}"
                          :cell-style="{textAlign: 'center'}"
                          style="width: 100%;font-size: 12px;color:black;font-weight: normal;">
                  <el-table-column label="基本信息">
                    <el-table-column prop="staffName" label="姓名" width="100"/>
                    <el-table-column prop="deptName" label="部门" width="100"/>
                    <el-table-column prop="positionName" label="职位" width="100"/>
                    <!--            <el-table-column   prop="name" label="本月变动" width="100" />-->
                  </el-table-column>
                  <!--        <el-table-column label="考勤数据" >
                            <el-table-column prop="name" label="应出勤天数" width="100" />
                            <el-table-column prop="name" label="实际出勤天数" width="100" />
                            <el-table-column prop="name" label="月计薪天数" width="100" />
                          </el-table-column>-->

                  <el-table-column prop="name" label="基本工资" width="110">
                    <template #default="scope">
                      <span v-if="scope.row.staffState==2" >{{scope.row.fixedwagePeriodmoney}}</span>
                      <span v-if="scope.row.staffState==3" >{{scope.row.fixedwageOfficialmoney}}</span>

                    </template>
                  </el-table-column>
                  <!--          <el-table-column prop="name" label="实发固定工资" width="100" />-->
                  <el-table-column label="加班工资">
                    <el-table-column prop="workdayMoney" label="工作日加班工资" width="110"/>
                    <el-table-column prop="restMoney" label="休息日加班工资" width="110"/>
                    <el-table-column prop="holidaysMoney" label="节假日加班工资" width="110"/>
                  </el-table-column>
                  <el-table-column prop="evection" label="出差" width="100"/>
                  <el-table-column prop="totalWages" label="工资合计" width="100"/>
                  <el-table-column label="考勤扣款">
                    <el-table-column prop="tardy" label="迟到" width="100"/>
                    <el-table-column prop="leave" label="早退" width="100"/>
<!--                    <el-table-column prop="name" label="未签到" width="100"/>-->
<!--                    <el-table-column prop="name" label="未签退" width="100"/>-->
                    <el-table-column prop="absenteeism" label="旷工" width="100"/>
                    <el-table-column prop="matterLeave" label="事假" width="100"/>
                    <el-table-column prop="fallLeave" label="病假" width="100"/>
                    <!--          <el-table-column prop="name" label="不在职免发" width="100" />-->
                  </el-table-column>
                  <el-table-column label="社保">
                    <el-table-column prop="insDetailSocialPersonPay" label="个人缴纳社保" width="100"/>
                    <el-table-column prop="insDetailSocialFirmPay" label="公司缴纳社保" width="100"/>
                  </el-table-column>
                  <el-table-column label="公积金">
                    <el-table-column prop="insDetailFundPersonPay" label="个人缴纳公积金" width="110"/>
                    <el-table-column prop="insDetailSocialFirmPay" label="公司缴纳公积金" width="110"/>
                  </el-table-column>
                  <el-table-column prop="wagesShould" label="应发工资" width="100" fixed="right"/>
                  <el-table-column prop="payroll" label="实发工资" width="100" fixed="right"/>
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
                    @size-change="selectUsers"
                    @current-change="selectUsers"
                >
                </el-pagination>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  &nbsp;
</template>


<script lang="ts">
export default {
  data() {
    return {
      //存工资数据
      tableData: [],
      seek: "",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },



    }
  },methods:{
    //查询薪酬组
    selectWag() {

      this.axios
          .get("http://localhost:8010/provider/WageVo/selectWageVo/" + this.pageInfo.currentPage + "/" + this.pageInfo.pagesize+"/"+this.$route.query.compId)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectWag()
  }
}
</script>
<style scoped>
@import url("../../css/navigation.css");

/deep/ .el-card__body {
  /* padding: var(--el-card-padding); */
  padding: 10px 0;
}


/deep/ .icon-p p {
  text-align: center;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}

.staff_div_5 {
  width: 204px;
  height: 80px;
  border: 1px solid #f0f0f0;
  display: inline-block;
  border-radius: 5px;
  position: relative;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);


}

.div_5_margin {

  margin-left: 5px;
}

.staff_div_5_text {
  width: 80px;
  height: 50px;
  /*background: #5cbdaa;*/
  display: inline-block;
  position: absolute;
  left: 105px;
  top: 20px;
  text-align: right;
  font-weight: bold;
  color: #085fc3;

}

</style>
