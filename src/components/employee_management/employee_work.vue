<!--员工花名册工作经历-->
<template>
<br/>
<div>
<!--搜索输入框-->
    <el-row style="width:200px;float:right;">
      <el-input v-model="input3" placeholder="搜索">
        <template #suffix>
          <el-icon class="el-input__icon"><i-search/></el-icon>
        </template>
      </el-input>
    </el-row>
</div>
  <div style="width:100px;height:30px;margin-top:40px;"></div>
<div>


  <el-table :data="tableData"  style="width: 100%"
            :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
            :cell-style="{textAlign: 'center'}">
    <el-table-column prop="staffName" label="姓名" width="210" />
    <el-table-column prop="workStareTime" label="开始时间" width="210" />
    <el-table-column prop="createdTime" label="结束时间" width="210" />
    <el-table-column prop="companyName" label="任职公司" width="210" />
    <el-table-column prop="positionName" label="职位" width="210" />

<!--    <el-table-column prop="name" label="离职原因" width="180" />-->
    <el-table-column fixed="right" label="操作">
    <template #default="scope">
      <router-link :to="{path:this.information,query:{path: this.$route.query.path}}">
			<el-button type="text" size="small" @click="empMsg(scope.row.staffId)">编辑 </el-button>
      </router-link>
	</template>
    </el-table-column>
  </el-table>
</div>

  <!-- 分页插件 -->
  <div class="demo-pagination-block">
    <el-pagination
        v-model:currentPage="pageInfo.currentPage"
        :page-sizes="[3, 5, 10, 50]"
        :page-size="pageInfo.pagesize"
        :default-page-size="pageInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="pageInfo.total"
        :pager-count="5"
        background
        @size-change="selectWork"
        @current-change="selectWork"
    >
    </el-pagination>
  </div>

</template>

<script lang="ts">
export default {
  data() {
    return {
      information:'/employee/message/employee_roster/information',
      tableData: [],
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      input3:"",

    }
  },
  methods:{

    //多表查询
    selectWork() {

      this.axios
          .get("http://localhost:8010/provider/workExperience/selectWorkExperience/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
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
    //根据id查询员工信息
    empMsg(staffId) {
      // 将当前行的员工id 存入 store里面，使得在其他页面也能取到
      this.$store.state.staffId_Msg = staffId;
      // 跳转页面
      this.$router.push({path: this.staffedit, query: {path: this.$route.query.path}})
    }
    },
  created() {
    this.selectWork()
  }

}

</script>
<style scoped>
/deep/.cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

</style>