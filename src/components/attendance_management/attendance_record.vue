<!--考勤记录-->
<template>
  <!--  考勤记录页面-->
  <div class="head">
    <div class="head-right">
      <!--  搜索框-->
      <el-input v-model="input3" placeholder="填写需要查询的！" style="width: 300px;margin-left: 10px;">
        <template #prepend>
          <el-select v-model="select" placeholder="查询" style="width: 80px">
            <el-option label="名称" value="名称"></el-option>
            <el-option label="编号" value="编号"></el-option>
          </el-select>
        </template>
        <template #append>
          <el-button>
            <el-icon style="font-size: 16px">
              <i-search/>
            </el-icon>
          </el-button>
        </template>
      </el-input>

      <!--导出导入-->
      <el-button type="warning" plain size="small" style="margin-left: 700px">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>

      <el-button type="success" plain size="small">
        <i class="iconfont">&#xe645;</i>
        导入
      </el-button>
      <el-button type="info" plain size="small"> 历史归档</el-button>
    </div>
    <div class="select">
      <span style="margin-top:2px">部门：</span>
      <!-- 查询所有部门-->
      <el-checkbox v-model="checked1" label="人事部" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="行政部" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="市场部" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="运营部" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="后勤部" size="medium"></el-checkbox>
    </div>
    <div class="selects">
      <span style="margin-top:2px">考勤状态：</span>
      <el-checkbox v-model="checked1" label="正常" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="旷工" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="迟到" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="早退" size="medium"></el-checkbox>
      <el-checkbox v-model="checked2" label="事假" size="medium"></el-checkbox>
    </div>
  </div>
  <!--表格-->
  <div class="bottom">
    <el-table :data="tableData" stripe border style="width: 100%;"
              :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
              :cell-style="{textAlign: 'center'}">
      <el-table-column prop="A" fixed label="序号" width="60"/>
      <el-table-column prop="B" fixed label="名称" width="100"/>
      <el-table-column prop="C" fixed label="部门" width="100"/>
      <el-table-column prop="q" label="6/1"/>
      <el-table-column prop="w" label="6/2"/>
      <el-table-column prop="w" label="6/3"/>
      <el-table-column prop="w" label="6/4"/>
      <el-table-column prop="w" label="6/5"/>
      <el-table-column prop="w" label="6/6"/>
      <el-table-column prop="w" label="6/7"/>
      <el-table-column prop="q" label="6/8"/>
      <el-table-column prop="q" label="6/9"/>
      <el-table-column prop="w" label="6/10"/>
      <el-table-column prop="q" label="6/11"/>
      <el-table-column prop="w" label="6/12"/>
      <el-table-column prop="q" label="6/13"/>
      <el-table-column prop="w" label="6/14"/>
      <el-table-column prop="q" label="6/15"/>
      <el-table-column prop="1" label="6/16"/>
      <el-table-column prop="1" label="6/17"/>
      <el-table-column prop="1" label="6/18"/>
      <el-table-column prop="1" label="6/19"/>
      <el-table-column prop="1" label="6/20"/>
      <el-table-column prop="1" label="6/21"/>
      <el-table-column prop="1" label="6/22"/>
      <el-table-column prop="1" label="6/23"/>
      <el-table-column prop="1" label="6/24"/>
      <el-table-column prop="1" label="6/25"/>
      <el-table-column prop="1" label="6/26"/>
      <el-table-column prop="1" label="6/27"/>
      <el-table-column prop="1" label="6/28"/>
      <el-table-column prop="1" label="6/29"/>
      <el-table-column prop="1" label="6/30"/>
    </el-table>
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
      input3: "",
      select: "",
      tableData: [
        {
          A: "1",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        },
        {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        },
        {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        },
        {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        },
        {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        }, {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        }, {
          A: "2",
          B: "王鑫",
          C: "人事部",
          q: "迟到",
          w: "√",
        },


      ],
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


.head-right {
  margin-top: 10px;
}

.select {
  margin-left: 15px;
  margin-top: 10px;
  color: #000000;
}

.selects {
  margin-left: 15px;
  margin-top: 10px;
  margin-bottom: 10px;
  color: #000000;
}

.bottom {
  margin-top: 27px;
  margin-left: 20px;
  margin-right: 5px;
  margin-bottom: 10px;
  padding: 1%;
}



.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 10px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff2?t=1640334127364') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff?t=1640334127364') format('woff'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.ttf?t=1640334127364') format('truetype');
}
</style>