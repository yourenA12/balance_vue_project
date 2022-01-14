<!--历史归档-->
<template>
  <!--  员工考勤历史归档页面-->
  <div class="head">
    <div class="head-to">
      <el-tabs v-model="activeName">
        <el-tab-pane name="first">
          <template #label>
            <span style="font-size: 24px"> 员工考勤历史归档 </span>
          </template>
        </el-tab-pane>
      </el-tabs>
      <!--  选择月份-->
      <el-date-picker v-model="value2" type="month" placeholder="请选择月份！">
      </el-date-picker>
      <!--导出导入-->
      <el-button type="warning" plain size="small" style="margin-left: 770px;margin-right: 20px">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>

      <el-input size="small" v-model="input" placeholder="请输入员/部名称" style="width:150px;">
      </el-input>

      <!--查询按钮-->
      <el-button style="background-color: #ffffff;border-radius: 30%; margin-left: 10px" size="small">
        <el-icon><i-search />
        </el-icon>
      </el-button>

    </div>
    <div class="centre">
      <el-icon style="font-size: 16px; color: #409eff"
      >
        <i-arrow-right-bold
        />
      </el-icon>
      <span style="margin-left: 40px">2021-6月份员工报表</span>
      <span style="margin-left: 750px; color: rgb(148, 140, 140)"
      >总人数：</span
      >
      <span>14</span>
      <span style="margin-left: 100px; color: rgb(148, 140, 140)"
      >全勤人数:</span
      >
      <span>1</span>
    </div>
    <div class="amei"></div>
    <div class="tis">
      <el-alert
          title="迟到、早退和补打卡的统计单位为 “次”; 所有假期类型、出差、旷工的统计单位均为 “天” 。 "
          type="warning"
          show-icon
      >
      </el-alert>
    </div>
    <!-- 表格-->
    <div class="bottom">
      <el-table :data="tableData" stripe border style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">
        <el-table-column prop="A" fixed label="名称"/>
        <el-table-column prop="B" fixed label="部门"/>
        <el-table-column prop="C" label="正常次数"/>
        <el-table-column prop="D" label="迟到次数"/>
        <el-table-column prop="D" label="早退次数"/>
        <el-table-column prop="D" label="旷工次数"/>
        <el-table-column prop="C" label="是否全勤"/>
      </el-table>
    </div>
  </div>

  <!--分页-->
  <div class="demo-pagination-block">
    <el-pagination
        v-model:currentPage="pageInfo.currenPage"
        :page-sizes="[3, 5, 10, 50]"
        v-model:page-size="pageInfo.pagesize"
        :default-page-size="pageInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="pageInfo.total"
        :pager-count="5"
        background
        @size-change="sele"
        @current-change="sele"
    >
    </el-pagination>
  </div>

</template>

<script>
export default {
  data() {
    return {
      activeName: "first",
      value2: "",

      /* 当前的页 */
      pageInfo: {
        currenPage: 1,
        pagesize: 3,
        total: 0,
      },
      // 历史归档数据
      tableData: [
        {
          A: '王鑫',
          B: '人事部',
          C: '1',
          D: '2',
        }, {
          A: '王鑫',
          B: '人事部',
          C: '1',
          D: '2',
        }, {
          A: '王鑫',
          B: '人事部',
          C: '1',
          D: '2',
        },

      ]

    };
  },
};
</script>

<style scoped>
.head {
  margin-top: 7px;
  margin-left: 20px;
  margin-right: 5px;
  margin-bottom: 10px;
}


.head-to {
  margin-left: 20px;
  margin-top: 15px;
}

.centre {
  margin-left: 20px;
  margin-top: 20px;
}

.amei {
  height: 2px;
  background-color: rgb(9, 152, 248);
  margin-top: 20px;
  margin-left: 20px;
}
.tis{
  margin-left: 20px;
}

.bottom {
  margin-top: 30px;
  margin-left: 20px;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 5px;
}
</style>