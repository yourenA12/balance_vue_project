<template>
  <!--  离职审批页面-->
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
                  @confirm="updateAuditflowdetai(scope.row.auditflowId,scope.row.auditflowdetailId,2,1)"
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
                  @confirm="updateAuditflowdetai(scope.row.auditflowId,scope.row.auditflowdetailId,2,666)"
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
          >
            <!--  @size-change="selectUsers" @current-change="selectUsers" -->
          </el-pagination>
        </div>
      </el-tab-pane>
      <!-- 点击详情弹出抽屉-->
      <el-drawer v-model="drawer" :with-header="false" title="I am the title">
        <span>

          <el-form :model="auditflow0" label-width="">
            <el-form-item label="员工名称 :">
              <el-input v-model="auditflow0.staffName1" disabled></el-input>
            </el-form-item>
             <el-form-item label="审核人名称 :">
              <el-input v-model="auditflow0.staffName2" disabled></el-input>
            </el-form-item>
                <el-form-item label="申请离职时间 :">
              <el-input v-model="auditflow0.applyQuitDate" disabled></el-input>
            </el-form-item>
                <el-form-item label="离职部门 :">
              <el-input v-model="auditflow0.deptName" disabled></el-input>
            </el-form-item>
            <el-form-item label="离职原因 :">
              <el-input v-model="auditflow0.quitExplain" disabled></el-input>
            </el-form-item>

          </el-form>

          <!-- process-status="error" -->
        <el-steps align-center :space="200" :active="active" finish-status="success">
          <el-step :status="statusa" :title="a" ></el-step>
          <el-step :status="statusb" :title="b" ></el-step>
          <el-step :status="statusc" :title="c"></el-step>
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
              v-model:currentPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
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
import {ElMessage} from "element-plus";

export default {
  setup() {
    return {
      drawer: ref(false),
      input: ref(""),
    };
  },
  data() {
    return {
      active:"",
      // 待办转正审批列表
      tableData: [],
      // 已办转正审批列表
      tableData1: [],

      pageInfo: {
        staffId:this.$store.state.userMsg.staffId,
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
        // 当前审批类型
        auditflowTitle:"离职审批",
        // 当前审批状态
        auditflowdetaiState:"待办",
        // 名称搜索框
        staffName:""
      },
      // 参数
      paramsVal:"",
      //已办申请分页插件
      pageInfo1: {
        staffId:this.$store.state.userMsg.staffId,
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
        auditflowTitle:"离职审批", // 当前审批类型
        auditflowdetaiState:"已办", // 当前审批状态
        staffName:""// 名称搜索框
      },
      auditflow: [],
      auditflow0: {},
      //那几个审批人
      a:{},
      b:{},
      c:{},

      statusa:"",
      statusb:"",
      statusc:"",
    };
  },
  methods: {
    //修改离职状态
    updateAuditflowdetai(id,mxid,state,isStaffState) {
      this.axios({
        url: 'http://localhost:8010/provider/auditflowdetail/updateAuditflowdetail',
        method: 'put',
        data:{
          auditflowId:id,
          auditflowdetailId:mxid,
          auditflowdetaiState:state,
          isStaffState:isStaffState
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '操作成功',
            type: 'success',
          })
          this.selectAuditflow(1) // 修改完成后调用查询方法
        } else {
          ElMessage.error('操作失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //查询
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
    //更具id查询离职详情
    selectById(row) {
      //打开抽屉
      this.drawer = true
      //根据id查询
      this.axios
          .get("http://localhost:8010/provider/findSelectByid/" + row.auditflowId)
          .then((response) => {
            console.log(response);
            this.auditflow0 = response.data.data[0]
            // this.auditflow0.staffName2=row.staffName2//绑定当前审批人
            this.auditflow = response.data.data;

            this.activeVal(row.staffName2)
          })
          .catch(function (error) {
            console.log(error);
          })
    },
    activeVal(aa){
      console.log("111111111111")
      console.log(this.auditflow)
      this.a = this.auditflow[0].staffName2
      this.b = this.auditflow[1].staffName2
      this.c = this.auditflow[2].staffName2
      let q=this.auditflow[0]
      let w=this.auditflow[1]
      let e=this.auditflow[2]
      if(q.auditflowdetaiState==1){
        this.active=0
      }
      if(w.auditflowdetaiState==1){
        this.active=1
        this.statusa="success"
      }
      if(e.auditflowdetaiState==1){
        this.active=2
        this.statusa="success"
        this.statusb="success"
      }
      if(q.auditflowdetaiState==2 && w.auditflowdetaiState==2 && e.auditflowdetaiState==2){
        this.active=3
        this.statusa="success"
        this.statusb="success"
        this.statusc="success"
      }
      if(q.auditflowdetaiState==3){
        this.active=0
        this.statusa="error"
      }else if(w.auditflowdetaiState==3){
        this.active=1
        this.statusa="success"
        this.statusb="error"
      }else if(e.auditflowdetaiState==3){
        this.active=2
        this.statusa="success"
        this.statusb="success"
        this.statusc="error"
      }
      this.auditflow0.staffName2=aa
    },
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
  },created() {
    this.selectAuditflow(1);
  }
};
</script>

<style scoped>
@import url("../../css/Examine_2.css");

</style>