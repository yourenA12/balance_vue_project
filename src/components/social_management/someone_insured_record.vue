<template>

<!-- 参保记录 -->
  <div style="margin-top: 10px">
    <!-- 表格按钮部分 -->
    <div class="mt-20 ml-20 mr-20" style="margin-bottom: 10px">
      <!-- 导出按钮 -->
      <el-button size="small"><i class="iconfont">&#xe6a2;</i>导出</el-button>

      <!-- 搜索框 -->
      <el-input v-model="search" size="small" class="resume-operation" placeholder="搜索">
        <template #suffix>
          <el-icon class="el-input__icon"><i-search /></el-icon>
        </template>
      </el-input>
    </div>

    <!-- 表格内容部分 -->
    <div class="sub-Content__primary">
      <el-table :data="tableData" style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f0f0f0',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
        <el-table-column prop="insuredMonth" label="计薪月份" />
        <el-table-column prop="insDetailSocialPersonPay" label="社保个人缴费" />
        <el-table-column prop="insDetailSocialFirmPay" label="社保公司缴费" />
        <el-table-column prop="insDetailFundPersonPay" label="积金个人缴费" />
        <el-table-column prop="insDetailFundFirmPay" label="积金公司缴费" />

        <el-table-column prop="cz" label="操作">
          <template #default="scope">
              <el-button @click="toLook(scope.row.insuredMonth)" type="text" size="small">查看 </el-button>
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
        @size-change="selectInsuredDetail"
        @current-change="selectInsuredDetail"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import { ref, defineComponent } from "vue";

export default {
  data() {
    return {
      // 搜索框
      search:"",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      tableData: [],
    };
  },
  methods:{

    toLook(date){

      // 传过来的月份
      this.$store.state.insuredMsg.date=date
      // 跳转页面
      this.$router.push({path:"/social/social_payment/someone_insured_particulars",query:{path:this.$route.query.path}})

    },

    //根据id查询参保明细
    selectInsuredDetail() {

      this.axios
          .get("http://localhost:8010/provider/insuredDetail/selectDInsuredbyId/" + this.pageInfo.currentPage + "/" + this.pageInfo.pagesize+"/"+this.$store.state.insuredMsg.staffId)
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
    this.selectInsuredDetail()
  }
};
</script>

<style scoped>
/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

/**
	 * 下拉选择器样式
	 */
.resume-operation {
  text-align: right;
  padding-left: 8px;
  float: right;
  width: 120px;
}
</style>

