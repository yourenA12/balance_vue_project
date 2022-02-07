<!--奖罚记录-->
<template>
<!--  <div class="saas-main-content">-->
<!--    <div class="j-card j-card-bordered mainContent">-->
      <div class="j-card-body">

        <!--搜索输入框-->
        <div style="width:100%;height:50px;margin: auto;">
          <!--搜索输入框-->
          <div style="display: inline-block;margin-top:30px;">
            <span style="font-weight:bold;margin-left: 10px">员工名称</span>
            <el-row style="width:200px;display: inline-block;margin-left: 15px">
              <el-input v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>
            </el-row>
          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold">部门 </span>

            <el-select v-model="pageInfo.deptSearch" placeholder="请输入部门名称" style="width: 200px;margin-left: 15px">
              <el-option
                  v-for="item in deptNameAll"
                  :key="item.deptId"
                  :label="item.deptName"
                  :value="item.deptId"
              >
              </el-option>
            </el-select>

          </div>

          <el-button @click="selectEmps()" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button @click="replacement()" style="width: 80px;" >
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>
        </div>
        <div style="width:100%;margin-top: 50px;" class="icon-p">


          <div style="width:100%;height:100px;margin: auto;background: #42b983;">
            <div style="text-align: center;">
              <el-table :data="tableData"
                        :header-cell-style="{textAlign: 'center',background:'#f8f8f9'}"
                        :cell-style="{textAlign: 'center'}"
                        style="width: 100%;font-size: 12px;color:black;font-weight: normal;">
                <el-table-column label="荣誉与奖励">
                  <el-table-column prop="staffName" label="荣誉与奖励姓名" width="230"/>
                  <el-table-column prop="deptName" label="部门" width="230"/>
                  <el-table-column prop="createdTime" label="奖励日期" width="230"/>
                  <el-table-column prop="gloryName" label="颁发单位名称" width="230"/>
                    <el-table-column   prop="gloryRemark" label="备注" width="230" />
                </el-table-column>
                <el-table-column prop="name" label="操作" width="100" fixed="right">
                  <router-link :to="{path:this.information,query:{path: this.$route.query.path}}">
                    <el-button type="text" size="small">编辑</el-button>
                  </router-link>
                </el-table-column>
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
                    @size-change="selectGloryPunish"
                    @current-change="selectGloryPunish"
                >
                </el-pagination>
              </div><br/><br/>


              <el-table :data="tableData"
                        :header-cell-style="{textAlign: 'center',background:'#f8f8f9'}"
                        :cell-style="{textAlign: 'center'}"
                        style="width: 100%;font-size: 12px;color:black;font-weight: normal;">

                <el-table-column label="惩罚">
                  <el-table-column prop="staffName" label="惩罚人名称" width="170"/>
                  <el-table-column prop="deptName" label="部门" width="170"/>
                  <el-table-column prop="punishType" label="惩罚类型" width="170"/>
                  <el-table-column prop="punishCause" label=" 惩罚原因" width="190"/>
                  <el-table-column prop="punishUnit" label="惩罚单位" width="170"/>
                  <el-table-column prop="isRevocation" label=" 是否撤销" width="160"/>
                  <el-table-column prop="punishRemark" label="备注" width="180"/>
                </el-table-column>


                <el-table-column prop="name" label="操作" width="100" fixed="right">
                  <router-link :to="{path:this.information,query:{path: this.$route.query.path}}">
                  <el-button type="text" size="small">编辑</el-button>
                  </router-link>
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
                  @size-change="selectGloryPunish"
                  @current-change="selectGloryPunish"
              >
              </el-pagination>
            </div>
          </div>
        </div>
      </div>
<!--    </div>-->
<!--  </div>-->
  &nbsp;
</template>


<script lang="ts">
export default {
  data() {
    return {
      information:'/employee/message/employee_roster/information',
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
    //多表查询
    selectGloryPunish() {
      this.axios
          .get("http://localhost:8010/provider/GloryPunishVo/selectGloryPunishVo/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
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
    this.selectGloryPunish()
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

</style>
