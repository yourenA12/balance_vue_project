<!--职位管理-->
<template>
  <!--头-->
  <div class="head">
    <!--查询-->
    <b style="font-size: 18px">职位名称</b>
    <el-input size="small" v-model="input" placeholder="请输入职位名称：" style="width:200px; margin-left: 10px">
    </el-input>
    <!--查询按钮-->
    <el-button style="background-color: #ffffff;border-radius: 30%; margin-left: 20px" size="small">
      <el-icon><i-search />

      </el-icon>
    </el-button>

    <!--新增按钮-->
    <div class="head-surface">
      <el-button size="small" type="primary" plain @click="drawer = true">
        <el-icon><i-plus/></el-icon>
        新增
      </el-button>

    </div>
    <!--抽屉-->
    <el-drawer v-model="drawer" :with-header="false">
      <el-form ref="ruleForm"
               :model="fo"
               :rules="rules">


        <el-form-item prop="name">
          <template #label><b style="font-size:18px;">职位名称：</b></template>
          <el-input v-model="fo.name" placeholder="请输入职位名称：" style="width:200px; margin-left: 20px">
          </el-input>
        </el-form-item>
        <!--选择框-->
        <el-form-item prop="values1">
          <template #label><b style="font-size:18px;">所属部门：</b></template>
          <el-select
              v-model="fo.values1"
              clearable
              placeholder="请选择部门">
            <el-option
                v-for="item in optionss"
                :key="item.values"
                :label="item.labels"
                :value="item.values"
            >
            </el-option>
          </el-select>
        </el-form-item>

        <!--选择框-->
        <el-form-item prop="value2">
          <template #label><b style="font-size:18px;">负责人：</b></template>
          <el-select
              v-model="fo.value2"
              clearable
              placeholder="请选择负责人">
            <el-option
                v-for="item in functionary"
                :key="item.value"
                :label="item.label"
                :value="item.value"
            >
            </el-option>
          </el-select>
        </el-form-item>

        <div class="an">
          <el-button type="primary" @click="submitForm('ruleForm')">
            <el-icon>
              <i-copy-document/>
            </el-icon>
            <span>提交</span>
          </el-button>

          <el-button @click="resetForm('ruleForm')"  >
            <el-icon><i-close-bold /></el-icon>
            <span>取消</span>
          </el-button>
        </div>
      </el-form>
    </el-drawer>

    <!--  表格-->
    <div class="y">
      <el-table :data="tableData" stripe style="width: 100% ;"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
        <el-table-column prop="positionName" label="职位名称"/>
        <el-table-column prop="positionDescription" label="所属部门"/>
        <el-table-column prop="operate" label="操作">
          <template #default>
            <el-button type="text" size="small" @click="redact()">
              <el-icon><i-edit />
              </el-icon>
              修改
            </el-button>


            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through2()"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon><i-delete /></el-icon>删除
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
          @size-change="post"
          @current-change="post"
      >
      </el-pagination>
    </div>

  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
export default {
  data() {
    return {
      fo:{
        name:"",
        values1:"",
        value2:"",

      },
      //分页
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },

      //抽屉
      drawer: ref(false),
      tableData: [

        {
          name: '经理',
          department: '开发部',
          functionary:'王鑫',
        },
        {
          name: '董事长',
          department: '开发部',
          functionary:'大熊',
        },
        {
          name: '秘书',
          department: '开发部',
          functionary:'静香',
        },
        {
          name: '经理',
          department: '开发部',
          functionary:'王鑫',
        },
        {
          name: '经理',
          department: '开发部',
          functionary:'王鑫',
        },
        {
          name: '经理',
          department: '开发部',
          functionary:'王鑫',
        },

      ],

      //部门
      optionss: ref([
        {
          values: '大象',
          label: '大象',
        },
        {
          values: '老鼠',
          label: '老鼠',
        }
      ]),
      values: ref(''),

      //负责人
      functionary: ref([
        {
          value: '大象',
          label: '大象',
        },
        {
          value: '老鼠',
          label: '老鼠',
        }
      ]),
      value2: ref(''),
      //验证
      rules:{
        name: [
          {
            required: true,
            message: "请填写班次名字",
            trigger: "blur",
          },
        ],
        values1:[
          {
            required: true,
            message: "请选择该部门",
            trigger: "change",
          }
        ],
        value2:[
          {
            required: true,
            message: "请选择该负责人",
            trigger: "change",
          }
        ],
      }

    };
  },
  methods: {
    //提交
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //取消
    resetForm(formName) {
      this.$refs[formName].resetFields()
      this.radio1= ref('1'),
          this.drawer = false
    },
    //分页查询
    post(){
      this.axios
          .get("http://localhost:8010/provider/position/Po/"+this.pageInfo.currenPage+"/"+this.pageInfo.pagesize)
          .then((response)=>{
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error){
            console.log(error);
          })
    },
  },
  created() {
    this.post();
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
.an{
  margin-top: 20px;
  margin-left: 1%;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 30px;
}
</style>

