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
          <el-input v-model="fo.positionName" placeholder="请输入职位名称：" style="width:200px;">
          </el-input>
        </el-form-item>
        <!--选择框-->
        <el-form-item>
          <template #label><b style="font-size:18px;">所属部门：</b></template>
          <el-select v-model="deptId" multiple ref="vueSelect" @change="onchange()" @click="onclicks()">
            <el-option hidden></el-option>
            <el-option
                class="xxx"
                v-for="item in dept"
                :key="item.deptId"
                :label="item.deptName"
                :value="item.deptId"
            >
            </el-option>
            <el-tree :data="deptlists"
                     show-checkbox
                     :default-expand-all=true
                     :check-on-click-node=true
                     node-key="deptId"
                     :props="defaultProps" ref="tree" @check-change="handleCheckChange()" />
          </el-select>
        </el-form-item>

        <!--输入框-->
        <el-form-item prop="values2">
          <template #label><b style="font-size:18px;">职位说明：</b></template>
          <el-input
              v-model="fo.positionDescription"
              maxlength="30"
              placeholder="职位说明"
              show-word-limit
              type="textarea"
              style="margin-left: 10px"
          />
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
                <el-table-column type="expand">
                  <template #default="scope">

                    <span style="margin-left: 50px">职位说明：{{ scope.row.positionDescription }}</span><br>

                  </template>
                </el-table-column>

        <el-table-column prop="positionName" label="职位名称"/>
        <el-table-column prop="deptNames" label="所属部门"/>
        <el-table-column prop="operate" label="操作">
          <template #default="scope">
            <el-button type="text" size="small" @click="drawer = true,updateRow(scope.row)">
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
import {ElMessage} from "element-plus";

export default {
  data() {
    // 格式
    const defaultProps = {
      children: 'children',
      label: 'deptName',
      value:'deptId'
    }
    return {

      res:"",
      // 选中值1
      res1:"",
      // 选中值2
      res2:[],
      // 部门  文本框的值
      dept:[],
      deptId:[],
      // 格式
      defaultProps,
      //存放部门信息
      deptlists: [],


      fo:{
        positionName:"",
        positionDescription:"",

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
      tableData: [],


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

      }

    };
  },
  methods: {
    //提交
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.insertPost()
          this.fo.positionName=""
          this.fo.positionDescription=""
          this.deptId=""
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //取消
    resetForm(formName) {
      this.drawer = false
      this.$refs[formName].resetFields()
      this.fo.positionName=""
      this.fo.positionDescription=""
      this.deptId=""

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
    // 当文本框值发生变化时调用的方法
    onchange(){

      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys(this.deptId, false)
    },

    // 点击文本框时调用的方法
    onclicks() {

      // 取当前选择器中的复选框选项id
      this.res1 = this.$refs.tree.getCheckedKeys()
    },

    //节点选中状态发生变化时调用的方法
    handleCheckChange(data, checked, indeterminate) {

      //获取所有选中的节点 start
      this.res = this.$refs.tree.getCheckedNodes()

      // 取当前选择器中的复选框选项id
      this.res2 = this.$refs.tree.getCheckedKeys()
      // 清空部门
      this.dept = []
      // 清空选中的部门
      this.deptId = []
      let x = 0
      for (let i = 0; i < this.res.length; i++) {

        for (let j = 0; j < this.res.length; j++) {
          // 如果父id 不等于 id 就加入到数据中
          if (this.res[i].deptPid != this.res[j].deptId) {
            //并且是最后一个
            if (j == this.res.length - 1 && x == 0) {
              // 加入数据
              this.dept.push(this.res[i])
              // 赋值到文本框
              this.deptId.push(this.res[i].deptId)
            }

          } else {
            x = 1
          }
        }
        x = 0
      }
    },
    //查询部门名称
    selectDeptName() {
      this.axios
          .get("http://localhost:8010/provider/dept/selectAll")
          .then((response) => {
            console.log(response);
            this.deptlists = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },

    //新增职位
    insertPost() {
      this.axios({
        url: 'http://localhost:8010/provider/deptPost/add',
        method: 'post',
        data: {
          Post: {
            positionName:this.fo.name,
            positionDescription:this.fo.explain
          },
          DeptIds: this.res2,

        }
      }).then(response => {
        if (response.data.data =="成功") {
          ElMessage.success("添加成功")
          this.post() //添加成功后，在查询一次
          this.drawer=false
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });
    },
    //修改 提取当前行
    updateRow(row){
      this.fo=row
    },

  },
  created() {
    this.post();
    this.selectDeptName()
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
.xxx{
  display:none
}
</style>

