<!--部门管理-->
<template>
  <!--头-->
  <div class="head">
    <!--查询-->
    <b style="font-size: 18px">部门名称</b>
    <el-input size="small" v-model="input" placeholder="请输入部门名称：" style="width:200px; margin-left: 10px">
    </el-input>
    <!--选择框-->
    <b style="font-size: 18px;margin-left: 20px">状态&nbsp;&nbsp;&nbsp;&nbsp;</b>
    <el-select size="small" v-model="value" clearable placeholder="部门状态">
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
      <el-icon>
        <i-search/>

      </el-icon>
    </el-button>

    <!--新增按钮-->
    <div class="head-surface">
      <el-button size="small" type="primary" plain @click="drawer = true,this.fo={}">
        <el-icon>
          <i-plus/>
        </el-icon>
        新增
      </el-button>

    </div>

    <!--抽屉-->
    <el-drawer v-model="drawer" :with-header="false">
      <el-form ref="ruleForm"
               :model="fo"
               :rules="rules">
        <el-form-item prop="name">
          <template #label><b style="font-size:18px;">部门名称：</b></template>
          <el-input v-model="fo.deptName" placeholder="请输入部门名称：" style="width:200px; margin-left: 20px">
          </el-input>
        </el-form-item>
        <!--选择框-->
        <el-form-item prop="values1">
          <template #label><b style="font-size:18px;">部门负责人：</b></template>
          <el-input  @click="become=true" v-model="fo.staffName" placeholder="选择员工" style="width:200px; margin-left: 20px"/>
<!--          <el-select-->
<!--              v-model="fo.values1"-->
<!--              clearable-->
<!--              placeholder="请选择部门负责人">-->
<!--            <el-option-->
<!--                v-for="item in optionss"-->
<!--                :key="item.values"-->
<!--                :label="item.labels"-->
<!--                :value="item.values"-->
<!--            >-->
<!--            </el-option>-->
<!--          </el-select>-->

        </el-form-item>

        <!-- 弹出框 -->
        <div>
          <el-dialog
              v-model="become"
              title="选择员工"
              width="50%"
              :close-on-click-modal="false">

            <!--搜索输入框-->
            <el-row style="width: 200px;margin-left:528px;margin-top: -20px">
              <el-input v-model="seek2" placeholder="搜索">
                <template #suffix @click="become = true">
                  <el-icon class="el-input__icon">
                    <i-search/>
                  </el-icon>
                </template>
              </el-input>
            </el-row>

            <el-table
                :data="deptData"
                height="250"
                style="width: 100%;margin-top: 20px;"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}">

              <el-table-column width="60">
                <template #default="scope">
                  <el-radio :label="scope.row.staffId" v-model="radioStaff" @change.native="getCurrentRow(scope.row)">
                    <!-- 以为有Label的原因，所以添加&nbsp以空格显示 -->
                    &nbsp;
                  </el-radio>
                </template>

              </el-table-column>

              <el-table-column
                  prop="staffName"
                  label="姓名">
              </el-table-column>
              <el-table-column
                  prop="deptName"
                  label="性别">
              </el-table-column>
              <el-table-column
                  prop="staffName"
                  label="学历">
              </el-table-column>
              <el-table-column
                  prop="postName"
                  label="职位">
              </el-table-column>
            </el-table>

            <div style="margin-top: 30px;margin-left:280px">
              <el-button @click="become=false" style="width: 80px;">取消</el-button>
              <el-button type="primary" style="width: 80px;" @click="staffRow()">确定</el-button></div>


          </el-dialog>
        </div>

        <!--单选框-->
        <b style="font-size: 18px;margin-right: 30px">状态:</b>
        <el-radio v-model="fo.deptState" :label="0">启用</el-radio>
        <el-radio v-model="fo.deptState" :label="1">禁用</el-radio>
        <div class="an">

          <el-button @click="resetForm('ruleForm')">
            <el-icon>
              <i-close-bold/>
            </el-icon>
            <span>取消</span>
          </el-button>

          <el-button type="primary" @click="submitForm('ruleForm')">
            <el-icon>
              <i-copy-document/>
            </el-icon>
            <span>提交</span>
          </el-button>

        </div>
      </el-form>
    </el-drawer>
    <!--  表格-->
    <div class="y">
      <el-table :data="tableData"
                v-if="refreshTable"
                row-key="deptId"
                :default-expand-all="expands"
                @expand-change="expands=true"
                stripe style="width: 100% ;"
                :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                :cell-style="{textAlign: 'center'}"
      >
<!--        <el-table-column type="expand">-->
<!--          <template #default="scope">-->

<!--            <span style="margin-left: 50px">部门负责人：{{ scope.row.staffName }}</span><br>-->
<!--            <span style="margin-left: 50px">部门人数：{{ scope.row.empNum }}</span>-->

<!--          </template>-->
<!--        </el-table-column>-->

        <el-table-column prop="deptName" label="部门名称"/>
        <el-table-column prop="createdTime" label="创建时间"/>
        <el-table-column prop="deptState" label="状态">
          <template #default>
            <el-button size="mini" round type="primary" plain>启用</el-button>
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template #default="scope">



            <el-button type="text" size="small" @click="drawer = true,updateRow(scope.row)">
              <el-icon>
                <i-edit/>
              </el-icon>
              修改
            </el-button>


            <el-popconfirm
                confirm-button-text="确定"
                cancel-button-text="取消"
                :icon="InfoFilled"
                icon-color="red"
                title="确定删除吗?"
                @confirm="through3(scope.row)"
            >
              <template #reference>
                <el-button type="text" size="small">
                  <el-icon>
                    <i-delete/>
                  </el-icon>
                  删除
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
          @size-change="deptinquire()"
          @current-change="deptinquire()"
      >
      </el-pagination>
    </div>

  </div>
</template>

<script lang="ts">
import {ref, defineComponent} from "vue";
import {ElMessage} from "element-plus";

export default {
  data() {
    return {
      // 弹出框
      become:false,
      fo: {
        deptId:"",
        deptName: "",
        staffId: "",
        staffName: "",
        deptState: 1,

      },
      //分页
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      //单选状态
      radio1: ref('1'),

      //抽屉
      drawer: ref(false),
      tableData: [],
      //一键展开收缩菜单
      expands: false,
      //渲染展开菜单
      refreshTable: true,
      //状态
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

      //部门负责人
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
      deptData:[{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      },{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      },{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      },{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      },{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      },{
        staffName:'王鑫',
        deptName:'哈哈哈',
        postName:'还是你呀',
      }],
      //验证
      rules: {
        deptName: [
          {
            required: true,
            message: "请填写班次名字",
            trigger: "blur",
          },
        ],
        // values1: [
        //   {
        //     required: true,
        //     message: "请选择该负责人",
        //     trigger: "change",
        //   }
        // ]
      }

    };
  },
  methods: {

    //分页查询
    deptinquire() {
      this.axios
          .get("http://localhost:8010/provider/dept/yyds"/* + this.pageInfo.currenPage + "/" + this.pageInfo.pagesize*/)
          .then((rersponse) => {
            console.log(rersponse);
            this.tableData = rersponse.data.data;
            // console.log(rersponse.data.data.records)
            // this.pageInfo.total = rersponse.data.data.total;
          }).catch(function (error) {
        console.log(error);
      })
    },
    //删除
    deptdelete(row) {
      this.axios
          .delete("http://localhost:8010/provider/dept/deptsc/" + row.deptId)
          .then((response) => {
            console.log(response)
            if (response.data.data === "成功") {
              ElMessage.success("删除成功")
              this.deptinquire() //删除成功后，在查询一次
            } else {
              ElMessage.error("删除失败")
            }
          })

    },
    //添加
    add(){
      this.axios
          .post("http://localhost:8010/provider/dept/add", this.fo)
          .then((response) => {

            if( response.data.data ==="成功" ){
                ElMessage.success("添加成功")
              this.deptinquire() //删除成功后，在查询一次
            }else{
                ElMessage.error("添加失败")
            }
          }).catch(function (error){
            console.log(error);
          })
    },
    //修改 提取当前行
    updateRow(row){
      this.fo=row
    },
    //提交
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.add() //添加方法
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    //取消
    resetForm(formName) {
      this.$refs[formName].resetFields()
      this.radio1 = ref('1'),
          this.drawer = false
    },
    // 点击删除确认按钮触发
    through3(row) {
      this.deptdelete(row)
    },
    // 点击禁用确认按钮触发
    through4(row) {
      this.state(row)
    },
  },
  //分页查询
  created() {
    this.deptinquire();
  },
};
</script>

<style>
table * {
  text-align: center;
}

/* 调整输入框的高度 */
::v-deep .el-input__inner {
  height: 32px;
}

.head {
  margin-top: 20px;
}

.head-surface {
  margin-left: 20px;
  margin-top: 20px;
}

.y {
  margin-top: 10px;
}

.an {
  margin-top: 20px;
  margin-left: 1%;
}

.demo-pagination-block {
  float: right;
  margin-top: 20px;
  margin-bottom: 30px;
}
</style>

