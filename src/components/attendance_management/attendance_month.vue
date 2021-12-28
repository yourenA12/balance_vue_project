<!--考勤月报表-->
<template>
  <!-- 月份报表页面 -->
  <div class="head">
    <div class="head-surface">
      <span><span style="color:#000000">2021</span> 6月人事报表</span>
    </div>
    <div class="head-to">
      <el-tabs v-model="activeName">
        <el-tab-pane label="考勤统计" name="first"></el-tab-pane>
      </el-tabs>
    </div>
    <!--    搜索框-->
    <div class="head-examine">
      <el-input size="small" v-model="input" placeholder="请输入员/部名称" style="width:150px;">
      </el-input>

      <!--查询按钮-->
      <el-button style="background-color: #ffffff;border-radius: 30%; margin-left: 10px" size="small">
        <el-icon><i-search />
        </el-icon>
      </el-button>
      <!--导出导入-->
      <el-button type="warning" plain size="small" style="margin-left: 850px">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>
      <el-button type="info" plain size="small">
        归档2021 6月份报表
      </el-button>
    </div>
<!--表格-->
    <div class="bottom">
      <el-table :data="tableData" stripe border style="width: 100%" height="320"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">
        <el-table-column prop="A" fixed label="名称"/>
        <el-table-column prop="B" fixed label="部门"/>
        <el-table-column prop="C" label="正常"/>
        <el-table-column prop="D" label="迟到次数"/>
        <el-table-column prop="B" label="早退次数"/>
        <el-table-column prop="B" label="旷工次数"/>
        <el-table-column prop="B" label="是否全勤"/>
      </el-table>
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeName: "first",
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
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


.head-surface {
  font-size: 20px;
  text-align: center;
  margin-top: 20px;
}

.head-to {
  margin-left: 10px;
}

.head-examine {
  margin-left: 30px;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

.bottom {
  margin-left: 18px;
  margin-top: 20px;
  margin-right: 5px;
  margin-bottom: 10px;
  padding: 1%;
}

.demo-pagination-block {
  margin-left: 800px;
  margin-top: 20px;
  margin-bottom: 5px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff2?t=1640334127364') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff?t=1640334127364') format('woff'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.ttf?t=1640334127364') format('truetype');
}
</style>