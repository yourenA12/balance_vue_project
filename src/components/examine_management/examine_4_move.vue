<template>
  <!--  异动审批页面-->
  <div class="body_1">
    <el-tabs type="border-card">
      <!-- 待办申请页面 -->
      <el-tab-pane>
        <template #label>
          <span @click="selectAuditflow(1)">待办申请</span>
        </template>
        <el-button @click="resetDateFilter1">重置日期过滤</el-button>
        &nbsp;
        <el-input
            placeholder="输入名称搜索"
            v-model="pageInfo.staffName"
            style="width: 200px"
        />
        &nbsp;
        <el-button type="success" plain style="margin-bottom: 20px" @click="selectAuditflow(1)">搜索</el-button>
        <!--  表格 -->
        <el-table
            ref="filterTable1"
            row-key="date1"
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
              column-key="date1"
              :filters="[
              { text: '2016-05-01', value: '2016-05-01' },
              { text: '2016-05-02', value: '2016-05-02' },
              { text: '2016-05-03', value: '2016-05-03' },
              { text: '2016-05-04', value: '2016-05-04' },
            ]"
              :filter-method="filterHandler"
          />
          <el-table-column prop="auditflowId" label="审批编号" width="100"/>
          <el-table-column prop="auditflowTitle" label="流程" width="100"/>
          <el-table-column prop="staffName1" label="申请人" width="150"/>
          <!-- <el-table-column prop="name" label="操作人" width="100" /> -->
          <el-table-column prop="auditflowState" label="状态" width="100">
            <!-- 判断 prop的状态  -->
            <template #default="scope">

              <div v-if="scope.row.auditflowdetaiState=='0'">
                <div class="if_spz"></div>
                &nbsp;&nbsp;<span>待审</span>
              </div>
              <div v-if="scope.row.auditflowdetaiState=='1'">
                <div class="if_spz"></div>
                &nbsp;&nbsp;<span>待我审批</span>
              </div>


              <div v-if="scope.row.auditflowdetaiState=='2'">
                <div class="if_tg"></div>
                &nbsp;&nbsp;<span>通过</span>
              </div>

              <div v-if="scope.row.auditflowdetaiState=='3'">
                <div class="if_bh"></div>
                &nbsp;&nbsp;<span>驳回</span>
              </div>


            </template>

          </el-table-column>
          <el-table-column prop="staffName2" label="当前审批人" width="150"/>
          <el-table-column prop="updatedTime" label="最近处理" width="150"/>

          <el-table-column label="操作">
            <template #default="scope">
              <el-popconfirm
                  confirm-button-text="确定"
                  cancel-button-text="取消"
                  :icon="InfoFilled"
                  icon-color="red"
                  title="确定通过吗?"
                  @confirm="through1()"
              >
                <template #reference>
                  <el-button type="text">通过 </el-button>
                </template>
              </el-popconfirm>
              <el-popconfirm
                  confirm-button-text="确定"
                  cancel-button-text="取消"
                  :icon="InfoFilled"
                  icon-color="red"
                  title="确定驳回吗?"
                  @confirm="through2()"
              >
                <template #reference>
                  <el-button type="text">驳回 </el-button>
                </template>
              </el-popconfirm>

              <el-button type="text"   @click="selectById(scope.row)">详情 </el-button>

            </template>
          </el-table-column>
        </el-table>
        <!-- 分页插件 -->
        <br>
        <div class="demo-pagination-block" style="float: right;">
          <el-pagination
              v-model:currentPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="selectAuditflow(1)"
              @current-change="selectAuditflow(1)"
          >
            <!--  @size-change="selectUsers" @current-change="selectUsers" -->
          </el-pagination>
        </div>
      </el-tab-pane>

      <!-- 点击详情，弹出抽屉-->
      <el-drawer v-model="drawer" :with-header="false" title="I am the title">
        <span>
          <el-form :model="auditflow0" label-width="">
            <el-form-item label="员工名称 :">
              <el-input v-model="auditflow0.staffName1" disabled></el-input>
            </el-form-item>
             <el-form-item label="审核人名称 :">
              <el-input v-model="auditflow0.staffName2" disabled></el-input>
            </el-form-item>
                <el-form-item label="异动类型 :">
              <el-input v-model="auditflow0.transferType" disabled></el-input>
            </el-form-item>
                <el-form-item label="异动前部门名称 :">
              <el-input v-model="auditflow0.deptName" disabled></el-input>
            </el-form-item>
            <el-form-item label="异动后部门名称 :">
              <el-input v-model="auditflow0.deptName1" disabled></el-input>
            </el-form-item>
            <el-form-item label="异动前部门职位 :">
              <el-input v-model="auditflow0.positionName" disabled></el-input>
            </el-form-item>

          </el-form>

          <!-- process-status="error" -->
        <el-steps align-center :space="200" :active="active" finish-status="success">
          <el-step :title="a.staffName2" ></el-step>
          <el-step :title="b.staffName2" ></el-step>
          <el-step :title="c.staffName2"></el-step>
        </el-steps>

          <!--            <el-form-item :prop="auditflow[0].staffName" label="员工名称 :">-->
          <!--              -->
          <!--            <el-input   disabled></el-input>-->
          <!--          </el-form-item>-->
        </span>
      </el-drawer>
      <!-- 已办申请页面 -->
      <el-tab-pane>
        <template #label>
          <span @click="selectAuditflow(2)">已办申请</span>
        </template>
        <el-button @click="resetDateFilter">重置日期过滤</el-button>
        &nbsp;
        <el-input
            v-model="pageInfo1.staffName"
            placeholder="输入名称搜索nima"
            style="width: 200px"
        />
        &nbsp;
        <el-button type="success" plain style="margin-bottom: 20px">搜索</el-button>

        <el-table
            ref="filterTable"
            row-key="date"
            :data="tableData1"
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
              { text: '2016-05-01', value: '2016-05-01' },
              { text: '2016-05-02', value: '2016-05-02' },
              { text: '2016-05-03', value: '2016-05-03' },
              { text: '2016-05-04', value: '2016-05-04' },
            ]"
              :filter-method="filterHandler"
          />
          <el-table-column prop="auditflowId" label="审批编号" width="100"/>
          <el-table-column prop="auditflowTitle" label="流程" width="100"/>
          <el-table-column prop="staffName1" label="申请人" width="150"/>
          <!-- <el-table-column prop="name" label="操作人" width="100" /> -->
          <el-table-column prop="auditflowdetaiState" label="状态" width="100">
            <!-- 判断 prop的状态  -->
            <template #default="scope">

              <div v-if="scope.row.auditflowdetaiState=='2'" >
                <div class="if_tg"></div>
                &nbsp;&nbsp;<span>通过</span>
              </div>

              <div v-if="scope.row.auditflowdetaiState=='3'" >
                <div class="if_bh"></div>
                &nbsp;&nbsp;<span>驳回</span>
              </div>

              <div v-if="scope.row.auditflowdetaiState=='1'" >
                <div class="if_spz"></div>
                &nbsp;&nbsp;<span>待我审批</span>
              </div>

            </template>

          </el-table-column>
          <el-table-column prop="updatedTime" label="最近处理" width="140"/>
          <el-table-column label="操作">
            <template #default="scope">
              <el-button type="text"  @click="selectById(scope.row)">详情</el-button>
            </template>
          </el-table-column>
        </el-table>
        <!-- 分页插件 -->
        <br>

        <div class="demo-pagination-block" style="float: right;">
          <el-pagination
              v-model:currentPage="pageInfo1.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo1.pagesize"
              :default-page-size="pageInfo1.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo1.total"
              :pager-count="5"
              background
              @size-change="selectAuditflow(2)"
              @current-change="selectAuditflow(2)"
          >
            <!--  @size-change="selectUsers"
						@current-change="selectUsers" -->
          </el-pagination>
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
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
      // 待办转正审批列表
      tableData: [],
      // 已办转正审批列表
      tableData1: [],

      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        // 当前审批类型
        auditflowTitle:"异动审批",
        // 当前审批状态
        auditflowdetaiState:"待办",
        // 名称搜索框
        staffName:""

      },
      pageInfo1: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        // 当前审批类型
        auditflowTitle:"异动审批",
        // 当前审批状态
        auditflowdetaiState:"已办",
        // 名称搜索框
        staffName:""

      },
      auditflow0: {},
      a:{},
      b:{},
      c:{}
    };
  },
  methods: {
    // 重置日期过滤
    resetDateFilter1() {
      this.$refs.filterTable1.clearFilter("date1");
    },
    // 重置日期过滤
    resetDateFilter() {
      this.$refs.filterTable.clearFilter("date");
    },
    clearFilter() {
      this.$refs.filterTable.clearFilter();
    },
    selectAuditflow(val) {
      // 待办
      if(val==1){
        this.paramsVal=this.pageInfo
      }else{ // 已办
        this.paramsVal=this.pageInfo1
      }
      this.axios
          .get("http://localhost:8010/provider/findSelectPageWorker",{params:this.paramsVal})
          .then((response) => {
            console.log(response);
            if(val==1){ // 待办
              this.tableData = response.data.data.records;
              this.pageInfo.total = response.data.data.total;
            }else{ // 已办
              console.log(response);
              this.tableData1 = response.data.data.records;
              this.pageInfo1.total = response.data.data.total;
            }

          })
          .catch(function (error) {
            console.log(error);
          })
    },

    ///这里是人事异动  根据id 查询
    selectById(row) {
      //打开抽屉
      this.drawer = true
      //根据id查询
      this.axios
          .get("http://localhost:8010/provider/findSelectTranseferById/" + row.auditflowId)
          .then((response) => {
            console.log(response);
            this.auditflow = response.data.data;
            this.auditflow0 = this.auditflow[0]
            this.activeVal()
          })
          .catch(function (error) {
            console.log(error);
          })
    },activeVal(){
      this.a = this.auditflow[0]
      this.b = this.auditflow[1]
      this.c = this.auditflow[2]

      if(this.a.auditflowdetaiState==1)
        this.active=0
      if(this.b.auditflowdetaiState==1)
        this.active=1
      if(this.c.auditflowdetaiState==1)
        this.active=2
      if(this.a.auditflowdetaiState==2 && this.b.auditflowdetaiState==2 && this.c.auditflowdetaiState==2)
        this.active=3

    },
    // 筛选
    filterHandler(value, row, column) {
      const property = column["property"];
      return row[property] === value;
    },
    // 点击通过确认按钮触发
    through1() {
      alert(1)
    },
    // 点击驳回确认按钮触发
    through2() {
      alert(1)
    }
  }, created() {
    this.selectAuditflow(1);
  }
};
</script>

<style scoped>
@import url("../../css/Examine_2.css");

</style>