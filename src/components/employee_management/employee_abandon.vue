<!--  放弃入职员工页面  -->
<template>
  <div class="saas-main-content">

    <!--搜索输入框-->
    <el-row style="width:200px;float:right;">
      <el-input v-model="input3" placeholder="搜索">
        <template #suffix>
          <el-icon class="el-input__icon">
            <i-search/>
          </el-icon>
        </template>
      </el-input>
    </el-row>
    <div style="width:100px;height:30px;margin-top:40px;"></div>
    <el-table :data="tableData" style="width: 100%;"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}" >
      <el-table-column prop="resumeName" label="姓名" width="200"/>
      <el-table-column prop="deptName" label="部门" width="200"/>
      <el-table-column prop="postName" label="职位" width="200"/>
      <el-table-column prop="resumePhone" label="手机号" width="200"/>
      <el-table-column prop="hiredate" label="入职日期" width="200"/>
      <el-table-column prop="waiveReason" label="放弃原因" width=""/>

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
          @size-change="selectAbandon"
          @current-change="selectAbandon"
      >
      </el-pagination>
    </div>


  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      tableData: [],
      input3: '',
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },

    }
  },methods:{
    //多表查询
    selectAbandon() {
      this.axios
          .get("http://localhost:8010/provider/entryhirdeVo/selectEntryhirdeVoFQ/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    }
  },created() {
    this.selectAbandon();
  },
}
</script>


<style scoped>
/deep/ .mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

/deep/ .cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {

  float: right;
  margin: 20px;
}


/*.saas-main-content{*/
/*  position: relative;*/

/*}*/
</style>
