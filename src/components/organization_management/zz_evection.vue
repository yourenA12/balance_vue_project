<template>
  <!--头-->
  <div class="head">
    <!--查询-->
    <b style="font-size: 18px">部门名称</b>
    <el-input size="small" v-model="input" placeholder="请输入部门名称：" style="width:200px; margin-left: 10px">
    </el-input>
    <!--选择框-->
    <b style="font-size: 18px;margin-left: 20px">状态&nbsp;&nbsp;&nbsp;&nbsp;</b>
    <el-select size="small" v-model="value" clearable placeholder="班次状态">
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

    <!--新增按钮-->
    <div class="head-surface">
        <el-button size="small" type="primary" plain>
          <el-icon><i-plus/></el-icon>
          新增
        </el-button>
    </div>

    <!--  表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100%"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
        <el-table-column prop="name" label="部门名称"/>
        <el-table-column prop="times" label="创建时间"/>
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


  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
export default {
  data() {
    return {
      tableData: [
        {
          name: '开发部',
          times: '2020-12-12 12:34:23',

        },
        {
          name: '市场部',
          times: '2020-12-12 12:34:23',
        },
        {
          name: '人事部',
          times: '2020-12-12 12:34:23',
        },
        {
          name: '行政部',
          times: '2020-12-12 12:34:23',
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

            };
  },
};
</script>

<style>
table *{
  text-align: center;
}
/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

.head{
  margin-top: 20px;
}

.head-surface{
  margin-left: 20px;
  margin-top: 20px;
}

.y{
  margin-top: 10px;
}
</style>

