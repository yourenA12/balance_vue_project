<template>
  <!-- 参保方案 -->
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <!-- 表格按钮部分 -->
        <div class="mt-20 ml-20 mr-20">
          <!-- 新增参保方案按钮 -->
          <router-link :to="{path:this.path,query:{path:this.$route.query.path,name:'新增',id:''}}">
            <el-button style="width:75px" size="mini" type="primary" plain> +新增</el-button>
          </router-link>

          <el-button size="mini" plain class="search-ss" type="primary" @click="reset()">
            <i class="iconfont">
              &#xe6b8
            </i>
            重置
          </el-button>

          <el-button @click="selectAllPage()" size="mini" class="search-ss" type="primary">
            <i class="iconfont">
              &#xe61b
            </i>
            搜索
          </el-button>

          <!-- 下拉选择器 -->
          <div class="resume-operation">
            <el-select
                v-model="pageInfo.state"
                size="small"
                clearable
                placeholder="请选择公告状态"
            >
              <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              >
              </el-option>
            </el-select>
          </div>

          <!-- 方案名称搜索 -->
          <el-input v-model="pageInfo.input" style="width:200px;float: right;margin-right: 20px" size="small"
                    placeholder="请输入方案名称"></el-input>

        </div>


        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="tableData" style="width: 100%;margin-top: 20px"
                    :header-cell-style="{textAlign: 'center',background:'#f0f0f0',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column prop="defInsuredName" label="方案名称"/>
            <el-table-column prop="defInsuredNumber" label="参保人数"/>
            <el-table-column prop="defInsuredState" label="状态">
              <template #default="scope">
                {{ scope.row.defInsuredState==0?'启用':'禁用' }}
              </template>
            </el-table-column>
            <el-table-column label="操作">
              <template #default="scope"
              >
                <router-link :to="{path:this.path,query:{path:this.$route.query.path,name:'修改',id:scope.row.defInsuredId}}">
                  <el-button size="small" type="text">
                    编辑
                  </el-button>
                </router-link
                >&nbsp;

                <el-button @click="updateDefInsuredState(scope.row.defInsuredId,scope.row.defInsuredState)" type="text" size="small"> {{ scope.row.defInsuredState === 0 ? '禁用 ' : '启用 ' }}</el-button>

                <!-- 删除行确认框 -->
                <el-popconfirm v-if="scope.row.defInsuredState===1"
                               @confirm="deleteRow(scope.$index, scheme_table)" title="删除此方案?">
                  <template #reference>
                    <el-button style="color:red" type="text" size="small">删除</el-button>
                  </template>
                </el-popconfirm>


              </template>
            </el-table-column>
          </el-table>
        </div>

        <!-- 分页插件 -->
        <div class="demo-pagination-block">
          <el-pagination
              v-model:currentPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="selectAllPage()"
              @current-change="selectAllPage()"
          >
          </el-pagination>
        </div>
      </div>
    </div>
  </div>
  &nbsp;
</template>

<script>
import {ref, defineComponent} from "vue";
import {ElMessage} from 'element-plus'

export default {
  methods: {

    // 修改参保方案状态
    updateDefInsuredState(id,state){
      this.axios
          .put("http://localhost:8010/provider/defInsured/updateDefInsuredState",{defInsuredId:id,defInsuredState:state==0?1:0})
          .then((response) => {
            console.log(response);
            if(response.data.data>0){
              ElMessage({
                type: 'success',
                message: '操作成功！！',
              })
              // 调用查询
              this.selectAllPage()
            }else{
              ElMessage('操作失败！！')
            }
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 删除行
    deleteRow(id) {
      this.axios
          .delete("http://localhost:8010/provider/notice/deleteNotices/" +  id)
          .then((response) => {
            console.log(response);
            if(response.data.data>0){
              ElMessage({
                type: 'success',
                message: '删除成功！！',
              })
              // 调用查询
              this.selectAllPage()
            }else{
              ElMessage('删除失败！！')
            }
          })
          .catch(function (error) {
            console.log(error);
          });
    },

    // 重置按钮
    reset() {
      this.pageInfo.input = ""
      this.pageInfo.state = ""
      // 调用查询
      this.selectAllPage()
    },

    // 查询所有参保方案
    selectAllPage() {
      this.axios
          .get("http://localhost:8010/provider/defInsured/selectAllPage",{params:this.pageInfo})
          .then((response) => {
            console.log(response);
            this.tableData =response.data.data.records
            this.pageInfo.total=response.data.data.total
          })
          .catch(function (error) {
            console.log(error);
          });
    },

  },
  created() {
    // 调用查询
    this.selectAllPage()
  },
  data() {
    return {
      path: "/social/basic_setup/new_insured_scheme",
      pageInfo: {
        // 分页参数
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
        input: "",// 参保方案名称搜索框的值
        state: "",// 参保方案状态下拉框的值
      },
      //下拉选择器
      options: [
        {value: "0", label: "启用"},
        {value: "1", label: "禁用"},
      ],
      // 参保方案表数据
      tableData: [],
    };
  },
};
</script>

<style scoped>
/* 分页的样式 */
.demo-pagination-block {
  float: right;
  margin: 20px;
}

/**
	 * 下拉选择器样式
	 */
.resume-operation {
  float: right;
  width: 150px;
}

/* 外层阴影 */
.saas-main-content {
  padding-top: 12px;
  min-height: 500px;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.j-card:hover {
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  border-color: transparent;
}

.j-card-bordered {
  border: 1px solid #e9e9e9;
  border-top-color: rgb(233, 233, 233);
  border-right-color: rgb(233, 233, 233);
  border-bottom-color: rgb(233, 233, 233);
  border-left-color: rgb(233, 233, 233);
}

.j-card {
  background: #fff;
  border-radius: 4px;
  font-size: 14px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
  margin-top: 8px;
  min-height: 100%;
}

.j-card-body {
  padding: 2%;
}

table * {
  text-align: center;
}

.search-ss {
  float: right;
  margin-left: 20px;
}

</style>

