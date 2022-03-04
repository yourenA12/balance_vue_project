<template>
<!--  月度报表 -->
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <!-- 单个 -->
        <div class="main_div" v-for="(item,index) in tableData">
          <div class="interior_left_div">
            <span class="social_accumulation">
              <span>{{item.insuredMonth}}</span> 社保公积金</span
            >
            <span v-show="index!=0" style="font-size: 12px"> &nbsp;已归档</span>
            <br />
            <el-button type="text">导出参保明细</el-button>
            <el-button type="text">删除 </el-button>
          </div>

          <div class="interior_right_div">
            <div style="display: inline-block">
              <span style="margin: 35px">参保人数</span><br />
              <span style="margin: 35px">{{item.insuredNumber}}</span>
            </div>

            <div style="display: inline-block">
              <span style="margin: 35px">个人缴费</span><br />
              <span style="margin: 35px">{{item.individual}}</span>
            </div>

            <div style="display: inline-block">
              <span style="margin: 35px">企业缴费</span><br />
              <span style="margin: 35px">{{item.companies}}</span>
            </div>

            <div style="display: inline-block; margin-left: 20px">
                <el-button @click="toDetail(item.insuredMonth)" type="text">详情></el-button>
            </div>
          </div>
          <br />
          <!-- 分割线 -->
          <div class="cut_off"></div>
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
  &nbsp;
</template>

<script lang="ts">
export default {
  data() {
    return {
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
      tableData: [],
      multipleSelection: [],
    };
  },methods:{

    // 详情
    toDetail(date){

      this.$store.state.insuredMsg.date=date

      // 跳转页面
      this.$router.push({path:"/social/social_payment/payment_detail",query:{path:this.$route.query.path}})

    },

    //查询月度报表
    selectinsuredMonthVo() {

      this.axios
          .get("http://localhost:8010/provider/insuredMonthVo/selectinsuredMonthVo")
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectinsuredMonthVo()
  }
};
</script>

<style scoped>
/* 分割线 */
.cut_off {
  width: 100%;
  border-bottom: 1px solid #ededed;
}

/* 右div */
.interior_right_div {
  display: inline-block;
  float: right;
  margin-top: 10px;
}

/* 社保公积金 */
.social_accumulation {
  font-weight: bold;
  font-size: 16px;
}

/* 左div */
.interior_left_div {
  display: inline-block;
}

/* 大div */
.main_div {
  margin-bottom: 10px;
}

/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

/* 外层div 内边距 */
.j-card-body {
  padding: 20px 25px 20px 25px;
}

/* 外层阴影 */
.saas-main-content {
  padding-top: 12px;
  min-height: 500px;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}
.j-card:hover {
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  border-color: transparent;
}
.j-card-bordered {
  border: 1px solid #e9e9e9;
  border-top-color: rgb(233, 233, 233);
  border-right-color: rgb(233, 233, 233);
  border-bottom-color: rgb(233, 233, 233);
  border-left-color: rgb(233, 233, 233);
}
.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.j-card-body{
  padding:2%;
}

</style>
