<template>
<!-- 参保明细 -->
  <div class="outer-div">
    <!-- 标题 -->
    <div class="big-title">&nbsp;&nbsp;&nbsp;参保明细</div>
    <!-- 个人缴费信息 -->
    <div class="insured_details">
      <!-- 左 -->
      <div class="payment_details">
        <!-- 个人信息 -->
        <div style="float: left">
          <el-image class="picture" :src="url"></el-image>
        </div>
        <div style="margin-top: 20px">
          姓名：{{InsuredData.staffName}} <br />
          部门：{{InsuredData.deptName}} <br />
          职位：{{InsuredData.positionName}} <br /><br /><br />
          参保方案：{{InsuredData.defInsuredName}} <br />
          社保个人缴纳：{{InsuredData.insDetailSocialPersonPay}} <br />
          社保公司缴纳：{{InsuredData.insDetailSocialFirmPay}} <br />
          基金个人缴纳：{{InsuredData.insDetailFundPersonPay}} <br />
          基金公司缴纳： {{InsuredData.insDetailFundFirmPay}}<br />
          参保月份:{{InsuredData.insuredMonth}}
        </div>
        <el-popconfirm title="确定删除？" @confirm="deleteInsured()">
          <template #reference>
            <el-button v-show="this.$store.state.insuredMsg.date==this.$store.state.ymDate" type="text"> <span style="color:red" >删除</span>并调整 </el-button>&nbsp;&nbsp;
          </template>
        </el-popconfirm>

      </div>

      <!-- 右 -->
      <div class="payment_project">
        <!-- 缴纳明细表 -->
        <el-table :data="tableData" size="mini">
          <el-table-column prop="insuredPaymentType" label="缴纳项目" />
          <el-table-column prop="insuredPaymentNumber" label="基数" />

          <el-table-column label="个人缴纳">
            <el-table-column label="比例" >
              <template #default="scope">
                {{ scope.row.insArchivePersonProp }}%
              </template>
            </el-table-column>
            <el-table-column prop="insArchivePersonMoney" label="金额" />
          </el-table-column>

          <el-table-column prop="insArchivePersonSum" label="个人固定金额" />

          <el-table-column label="公司缴纳">
              <el-table-column label="比例" >
                <template #default="scope">
                  {{ scope.row.insArchiveFirmProp }}%
                </template>
              </el-table-column>
            <el-table-column prop="insArchiveFirmMoney" label="金额" />
          </el-table-column>

          <el-table-column prop="insArchiveFirmSum" label="公司固定金额" />

        </el-table>
      </div>
    </div>
<!--    &lt;!&ndash; 标题 &ndash;&gt;
    <div class="big-title">&nbsp;&nbsp;&nbsp;参保日志</div>

    <div class="outer-div">
      &lt;!&ndash; 参保日志  时间线 &ndash;&gt;
      <el-timeline>
        <el-timeline-item
          v-for="(activity, index) in activities"
          :key="index"
          :icon="activity.icon"
          :type="activity.type"
          :color="activity.color"
          :size="activity.size"
          :hollow="activity.hollow"
          :timestamp="activity.timestamp"
        >
          {{ activity.content }}
        </el-timeline-item>
      </el-timeline>
    </div>-->
  </div>
</template>

<script>
/* 时间线 */
import { defineComponent } from "vue";
import {ElMessage} from "element-plus";
// import { MoreFilled } from "@element-plus/icons-vue";

export default {
  setup() {
    return {
      /* 时间线 */
      activities: [
        {
          content: "Custom icon",
          timestamp: "2018-04-12 20:46",
          size: "large",
          type: "primary",
          //   icon: MoreFilled,
        },
        {
          content: "Custom color",
          timestamp: "2018-04-03 20:46",
          color: "#0bbd87",
        },
        {
          content: "Custom size",
          timestamp: "2018-04-03 20:46",
          size: "large",
        },
        {
          content: "Custom hollow",
          timestamp: "2018-04-03 20:46",
          type: "primary",
          hollow: true,
        },
        {
          content: "Default node",
          timestamp: "2018-04-03 20:46",
        },
      ],
    };
  },
  data() {
    return {
      // 查询参保详情明细
      tableData: [],
      //参保明细
      InsuredData:[],
    };
  },
  methods:{

    // 删除参保
    deleteInsured(){

      let ids = []
      ids.push(this.$store.state.insuredMsg.staffId)

      this.axios({
        url: 'http://localhost:8010/provider/insuredDetail/deleteInsuredAll/'+ids,
        method: 'DELETE',

      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '删除成功',
            type: 'success',
          })
          // 跳转页面
          this.$router.push({path: "/social/social_management/insured_management", query: {path: this.$route.query.path}})
        } else {
          ElMessage.error('删除失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

    },

    // 查询参保详情明细
    selectAllPage() {
      // 从store 中取出员工id进行查询
      this.axios
            .get("http://localhost:8010/provider/insuredDetailSon/selectDetailSonId/" + this.$store.state.insuredMsg.staffId+"/" +this.$store.state.insuredMsg.date )
            .then((response) => {
              console.log(response);
              this.tableData = response.data.data;

            })
            .catch(function (error) {
              console.log(error);
            });

    },
    //查询参保明细
    // 查询参保详情明细
    selectInsured() {
      // 从trore 中取出员工id进行查询
      this.axios
          .get("http://localhost:8010/provider/insuredDetail/selectDInsuredId/" + this.$store.state.insuredMsg.staffId+"/" +this.$store.state.insuredMsg.date )
          .then((response) => {
            console.error(response);
            this.InsuredData = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });

    },
  },
  created() {
    this.selectAllPage()
    this.selectInsured()

  }
};
</script>

<style lang="scss" scoped>
/* 个人头像 */
.picture {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}

/* 缴费项目 */
.payment_project {
  background-color: blue;
  display: inline-block;
  float: right;
  width: 73%;
}

/* 缴费信息 */
.payment_details {
  display: inline-block;
  float: left;
  width: 25%;
  margin-top: 50px;
}

/* 参保明细 */
.insured_details {
  text-align: center;
  height: 400px;
  margin: 0.5%;
}

/* 外层div */
.outer-div {
  padding: 10px 1% ;
}

/* 标题 */
.big-title {
  width: 98%;
  //height: 45px;
  background-color: #f2f4fa;
  font-size: 16px;
  font-weight: bold;
  padding: 1%;
  margin-bottom: 20px;
}
</style>
