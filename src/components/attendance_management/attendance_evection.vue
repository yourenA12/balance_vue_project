<!--出差记录-->
<template>
  <!--  出差查询页面-->
  <div class="w">
    <div class="head">
      <!--选择开始日期和结束日期-->
      <b style="font-size: 18px;margin-left:10px;margin-right: 10px">出差时间</b>
      <el-date-picker
          v-model="value1"
          type="daterange"
          unlink-panels
          range-separator="TO"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          wdaWD
          aW
          :shortcuts="shortcuts"
      >
      </el-date-picker>
      <!--    全部部门-->
      <b style="font-size: 18px;margin-left:25px;margin-right: 10px">全部部门</b>
      <el-select size="small" v-model="value" clearable placeholder="选择部门" >
        <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
        >
        </el-option>
      </el-select>
      <!--搜索框-->
      <b style="font-size: 18px;margin-left:25px;margin-right: 10px">员/部名称</b>
      <el-input size="small" v-model="input" placeholder="请输入员/部名称" style="width:150px;">
      </el-input>

      <!--查询按钮-->
      <el-button style="background-color: #ffffff;border-radius: 30%; margin-left: 20px" size="small">
        <el-icon><i-search />

        </el-icon>
      </el-button>
    </div>

    <div class="head-export">
      <!--导出导入-->
      <el-button type="warning" plain size="small" @click="derive()">
        <i class="iconfont">&#xe643;</i>
        导出
      </el-button>

      <el-button type="success" plain size="small">
        <i class="iconfont">&#xe645;</i>
        导入
      </el-button>
    </div>
    <!--表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">
        <el-table-column prop="applyfor" label="申请名称"/>
        <el-table-column prop="department" label="发起人部门"/>
        <el-table-column prop="place" label="出差地点"/>
        <el-table-column prop="thing" label="出差事由"/>
        <el-table-column prop="begin" label="出差开始时间"/>
        <el-table-column prop="finish" label="出差结束时间"/>
        <el-table-column prop="duration" label="出差时长"/>
        <el-table-column prop="operate" label="操作">
          <template #default>
            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through1()"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon><i-delete /></el-icon> 删除
                </el-button>
              </template>
            </el-popconfirm>
          </template>
        </el-table-column>
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

<script lang="ts">
import {ref, defineComponent} from "vue";
export default {
  data() {
    return {
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      //选择时间
      shortcuts: [
        {
          text: "最近一周",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
            return [start, end];
          },
        },
        {
          text: "最近一个月",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
            return [start, end];
          },
        },
        {
          text: "最近三个月",
          value: () => {
            const end = new Date();
            const start = new Date();
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
            return [start, end];
          },
        },
      ],
      //查询全部部门
      options: ref([
        {
          value: "Option1",
          label: "Option1",
        },
        {
          value: "Option2",
          label: "Option2",
        },
        {
          value: "Option3",
          label: "Option3",
        },
        {
          value: "Option4",
          label: "Option4",
        },
        {
          value: "Option5",
          label: "Option5",
        },
      ]),
      //查询出差数据
      tableData: [
        {
          applyfor: '傻逼',
          department: '人事部',
          place: '厦门',
          thing: '公司',
          begin: '12-8',
          finish: '12-18',
          duration: '10天',
        },
        {
          applyfor: '胡桃',
          department: '人事部',
          place: '厦门',
          thing: '公司',
          begin: '12-8',
          finish: '12-18',
          duration: '10天',
        },
        {
          applyfor: '胡桃',
          department: '人事部',
          place: '厦门',
          thing: '公司',
          begin: '12-8',
          finish: '12-18',
          duration: '10天',
        },

      ],
      value1: "", //日期
      value: ref(""), //选择
    };
  },
  methods: {
    // 点击删除确认按钮触发
    through1() {
      alert(1)
    },
  }
};
</script>

<style>

table * {
  text-align: center;
}


.y {
  margin-top: 20px;
}

.head {
  margin-top: 2%;
}

.demo-pagination-block {
  margin-left: 850px;
  margin-top: 20px;
  margin-bottom: 30px;
}

.head-export{
  margin-top: 20px;
  margin-left: 20px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff2?t=1640334127364') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.woff?t=1640334127364') format('woff'),
  url('//at.alicdn.com/t/font_3056280_a3nxem44wtk.ttf?t=1640334127364') format('truetype');
}

</style>

