<!--班次管理页面-->
<template>
  <div class="w">

    <!-- 搜索框 -->
    <div class="head">
    <b style="font-size: 18px">班次名称</b>
    <el-input v-model="input" placeholder="请输入班次名称：" style="width:200px; margin-left: 10px">
    </el-input>
      <b style="font-size: 18px;margin-left: 20px">班次状态&nbsp;&nbsp;&nbsp;&nbsp;</b>
      <el-select v-model="value" clearable placeholder="班次状态">
        <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
        >
        </el-option>
      </el-select>
      <!--查询按钮-->
      <el-button style="background-color: #ffffff;border-radius: 30%; margin-left: 20px" size="small">
        <el-icon><i-search />

        </el-icon>
      </el-button>


    </div>
    <!--新增按钮-->
    <div class="head-surface">
    <router-link :to="{path: this.one,query:{path:this.$route.query.path}}">
      <el-button size="small" type="primary" plain>
        <el-icon><i-plus/></el-icon>
        新增
      </el-button>
    </router-link>
    </div>
    <!--  表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
        <el-table-column prop="name" label="班次名称"/>
        <el-table-column prop="time" label="上班时间"/>
        <el-table-column prop="times" label="下班时间"/>
        <el-table-column prop="state" label="状态">
          <template #default>
            <el-button size="mini" round type="primary" plain>启用</el-button>
          </template>
        </el-table-column>
        <el-table-column prop="operate" label="操作">
          <template #default>
            <el-button type="text" size="small" @click="redact()">
              <el-icon><i-edit />
              </el-icon>修改
            </el-button>
            <span style="color:#e8e8e8">|</span>
            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定禁用吗?"
                @confirm="through1()"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon><i-warning /></el-icon>禁用
                </el-button>
              </template>
            </el-popconfirm>
            <span style="color:#e8e8e8">|</span>
            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through2()"
            >
              <template #reference>
                <el-button type="text" size="small" style="color:darkorange">
                  <el-icon><i-delete /></el-icon>删除
                </el-button>
              </template>
            </el-popconfirm>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!--  分页-->
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
import { ref, defineComponent } from 'vue'
export default {
  data() {
    return {
      // 新增路由地址
      one: '/attendance/check/classes/addclass',
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      tableData: [
        {
          name: 'Tom',
          time: '8:00',
          times: '18:00',

        },
        {
          name: 'Tom',
          time: '8:00',
          times: '18:00',
          state: '启用',
        },
        {
          name: 'Tom',
          time: '8:00',
          times: '18:00',
          state: '启用',
        },
        {
          name: 'Tom',
          time: '8:00',
          times: '18:00',
          state: '启用',
        }
      ],
      options: ref([
        {
          value: '启用',
          label: '启用',
        },
        {
          value: '禁用',
          label: '禁用',
        }
      ]),
      value: ref(''),
    }
  },
  methods: {
    // 点击编辑跳转
    redact() {
      this.$router.push({
        path: this.one,
        query: {path: this.$route.query.path}
      })
    },
    // 点击禁用确认按钮触发
    through1() {
      alert(1)
    },
    // 点击删除确认按钮触发
    through2() {
      alert(1)
    },
  }
}
</script>

<style scoped>
/*居中*/
table * {
  text-align: center;
}

.head{
  margin-left: 10px;
  margin-top: 30px;
}

.head-surface{
  margin-left: 20px;
  margin-top: 20px;
}
.y{
  margin-top: 10px;
}

.demo-pagination-block {
  margin-left: 850px;
  margin-top: 20px;
  margin-bottom: 30px;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

@font-face {
  font-family: 'iconfont';  /* Project id 3056280 */
  src: url('//at.alicdn.com/t/font_3056280_njgieohi6cg.woff2?t=1640333406121') format('woff2'),
  url('//at.alicdn.com/t/font_3056280_njgieohi6cg.woff?t=1640333406121') format('woff'),
  url('//at.alicdn.com/t/font_3056280_njgieohi6cg.ttf?t=1640333406121') format('truetype');
}
</style>