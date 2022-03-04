<template>
  <!--  我的申请页面-->
  <div class="body_1">
    <el-tabs type="border-card">
      <el-tab-pane label="我的申请">
        <el-button @click="resetDateFilter">重置日期过滤</el-button>
        &nbsp;
        <el-input
            v-model="input"
            placeholder="输入名称搜索"
            style="width: 200px"
        />
        &nbsp;
        <el-button type="success" plain style="margin-bottom: 20px">搜索</el-button>
        <!-- 表格   -->
        <el-table
            ref="filterTable"
            row-key="date"
            :data="tableData"
            style="width: 100%"
            :header-cell-style="{textAlign: 'center',background:'#f0f0f0',color:'#6C6C6C'}"
            :cell-style="{textAlign: 'center'}"

        >
          <el-table-column
              prop="createdTime"
              label="日期"
              sortable
              width="140"
              column-key="date"
              :filters="[
              { text: '2021-12-29', value: '2016-05-01' },
              { text: '2016-05-02', value: '2016-05-02' },
              { text: '2016-05-03', value: '2016-05-03' },
              { text: '2016-05-04', value: '2016-05-04' },
            ]"
              :filter-method="filterHandler"
          />
          <el-table-column prop="auditflowId" label="审批编号" width="100"/>
          <el-table-column prop="auditflowTitle" label="流程" width="100"/>

          <el-table-column prop="staffName1" label="申请人" width="100"/>
          <el-table-column prop="auditflowState" label="状态" width="100">
            <!-- 判断 prop的状态  -->
            <template #default="scope">

              <div v-if="scope.row.auditflowState=='2'" >
                <div class="if_tg"></div>
                &nbsp;&nbsp;<span>通过</span>
              </div>

              <div v-if="scope.row.auditflowState=='3'" >
                <div class="if_bh"></div>
                &nbsp;&nbsp;<span>驳回</span>
              </div>

              <div v-if="scope.row.auditflowState=='0'" >
                <div class="if_spz"></div>
                &nbsp;&nbsp;<span>待审</span>
              </div>

              <div v-if="scope.row.auditflowState=='1'" >
                <div class="if_cx"></div>
                &nbsp;&nbsp;<span>撤销</span>
              </div>
            </template>

          </el-table-column>

          <el-table-column prop="updatedTime" label="最近处理" width="100"/>
          <el-table-column label="操作">
            <template #default="scope">

              <el-button type="text"  @click="drawer = true">详情 </el-button>
            </template>
          </el-table-column>
        </el-table>

        <!-- 分页插件 -->
        <br>
        <div class="demo-pagination-block" style="float:right;">
          <el-pagination
              v-model:currentPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="Auditflow()"
              @current-change="Auditflow()"
          >
          </el-pagination>
        </div>

      </el-tab-pane>
    </el-tabs>
    <!--   弹出抽屉 -->
    <el-drawer v-model="drawer" title="I am the title" :with-header="false">
   <span>
          <el-form :model="auditflow0" label-width="">
            <el-form-item label="员工名称 :">
              <el-input v-model="auditflow0.staffName1" disabled></el-input>
            </el-form-item>
             <el-form-item label="审核人名称 :">
              <el-input v-model="auditflow0.staffName2" disabled></el-input>
            </el-form-item>
              <el-form-item label="发起时间 :">
              <el-input v-model="auditflow0.createdTime" disabled></el-input>
            </el-form-item>
            <el-form-item label="审批类型 :">
              <el-input v-model="auditflow0.auditflowTitle" disabled></el-input>
            </el-form-item>


          </el-form>

     <!-- process-status="error" -->



     <!--            <el-form-item :prop="auditflow[0].staffName" label="员工名称 :">-->
     <!--              -->
     <!--            <el-input   disabled></el-input>-->
     <!--          </el-form-item>-->
        </span>
    </el-drawer>
  </div>
</template>

<script scoped>
import {defineComponent, ref} from "vue";

export default {
  setup() {
    return {
      drawer: ref(false),
      input: ref(""),
    };
  },
  data() {
    return {
      tableData:[],
      auditflow0:[],
      active:"",
      auditflow:"",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
      },
    };
  },
  methods: {
    Auditflow(){
      this.axios
          .get("http://localhost:8010/provider/findByIdUser/"+this.$store.state.userMsg.staffId+"/"+this.pageInfo.currentPage+ "/" + this.pageInfo.pagesize)
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            this.pageInfo.total=response.data.data.total;
            this.auditflow0 = response.data.data.records[0];
          })
          .catch(function (error) {
            console.log(error);
          })

    },
    //重置过滤
    resetDateFilter() {
      this.$refs.filterTable.clearFilter("date");
    },
    clearFilter() {
      this.$refs.filterTable.clearFilter();
    },
    formatter(row, column) {
      return row.address;
    },
    filterTag(value, row) {
      return row.tag === value;
    },
    // 筛选
    filterHandler(value, row, column) {
      const property = column["property"];
      return row[property] === value;
    },
    //更具员工id查询 员工的申请


    // 点击撤销确认按钮触发
    through1() {
      alert(1)
    }
  }, created() {
  this.Auditflow();
}

};
</script>

<style scoped>
@import url("../../css/Examine_2.css");

</style>
