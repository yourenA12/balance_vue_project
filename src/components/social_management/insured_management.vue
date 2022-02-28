<template>

<!-- 参保人员管理 -->
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <div class="one">批量参保</div>

        <div class="all-input">
          参保方案:
          <el-select
              v-model="scheme_name"
              style="width: 300px"
              size="small"
              clearable
              placeholder="请选择"
              @change="sub"
          >
            <el-option
                v-for="item in insured_scheme"
                :key="item.defInsuredId"
                :label="item.defInsuredName"
                :value="item.defInsuredId"
            >
            </el-option>
          </el-select>
          <el-popover placement="top" :width="225" trigger="hover">
            <template #reference>
              <el-button type="text"
              ><i class="iconfont">&nbsp; &#xe619;</i></el-button
              >
            </template>
            <span style="color: #938c8c"
            >添加社保方案请前往<router-link :to="{path:this.path,query:{path:this.$route.query.path}}">参保方案</router-link
            >页</span
            >
          </el-popover>
          <br/><br/>

          <!-- 填写基本参数的表单 -->
<!--          <div style="text-align: center">
            &lt;!&ndash; left &ndash;&gt;
            <div class="form-div">
              <el-switch
                  class="form-seitch"
                  inline-prompt
                  active-text="开"
                  inactive-text="关"
              />

              <el-form :rules="rules">
                <el-form-item prop="name">
                  社保基数：
                  <el-input
                      v-model="name"
                      size="small"
                      placeholder="请输入社保基数"
                  />
                </el-form-item>
                <el-form-item prop="name">
                  参保月份：
                  <el-date-picker
                      size="small"
                      type="month"
                      placeholder="选择参保月份"
                  />
                </el-form-item>
                <el-form-item>
                  计薪月份：
                  <el-date-picker
                      size="small"
                      type="month"
                      placeholder="选择计薪月份"
                  />
                </el-form-item>
              </el-form>
            </div>

            &lt;!&ndash; right &ndash;&gt;
            <div class="form-div">
              <el-switch
                  class="form-seitch"
                  inline-prompt
                  active-text="开"
                  inactive-text="关"
              />

              <el-form :rules="rules">
                <el-form-item prop="name">
                  公积金基数：
                  <el-input
                      size="small"
                      placeholder="请输入公积金基数"
                  />
                </el-form-item>
                <el-form-item>
                  参保月份：
                  <el-date-picker
                      size="small"
                      type="month"
                      placeholder="选择参保月份"
                  />
                </el-form-item>
                <el-form-item>
                  计薪月份：
                  <el-date-picker
                      size="small"
                      type="month"
                      placeholder="选择计薪月份"
                  />
                </el-form-item>
              </el-form>
            </div>
          </div>-->
        </div>

        <!-- 表格按钮部分 -->
        <div class="mt-20 ml-20 mr-20">
          <!-- 提交按钮 -->
          <el-button style="width:75px" v-bind:disabled="disableds" size="small" type="primary" @click="submitAdd()" plain>提交 </el-button>

          <!-- 搜索框 -->
<!--          <el-input v-model="search" size="small" class="resume-operation" placeholder="搜索">-->
<!--            <template #suffix>-->
<!--              <el-icon class="el-input__icon">-->
<!--                <i-search/>-->
<!--              </el-icon>-->
<!--            </template>-->
<!--          </el-input>-->



          <el-input style="width: 200px;margin-left: 390px;" size="small" v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>

          <!-- 下拉选择器 -->
<!--          <div style="width: 200px" class="resume-operation">-->

            <el-select v-model="deptId" size="small" multiple ref="vueSelect" @change="onchange()" @click="onclicks()"  placeholder="选择部门" style="margin-left: 15px">
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

          <el-select placeholder="请选择状态" size="small" v-model="pageInfo.stateSearch" style="margin-left: 15px;">
            <el-option label="正式" value="3" style="margin-left: 15px"></el-option>
            <el-option label="试用" value="2" style="margin-left: 15px"></el-option>
          </el-select>
<!--          </div>-->

          <el-button @click="selectsocialStaffPage()" size="small" type="primary" style="width: 80px;margin-left:25px;margin-top: 20px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button @click="replacement()" size="small"  style="width: 80px;" >
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>


        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="emps_table" style="width: 100%"
                    @selection-change="tableChange"
                    :header-cell-style="{textAlign: 'center',background:'#f0f0f0',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}"
          >
            <!-- 多选框 -->
            <el-table-column type="selection" width="55"/>
            <el-table-column prop="staffName" label="姓名"/>
            <el-table-column prop="deptName" label="部门"/>
            <el-table-column prop="staffState" label="状态">
              <template #default="scope">
              <span style="color:#fa8c16;" v-if="scope.row.staffState==2">试用</span>
              <span style="color: #13c2c2;" v-if="scope.row.staffState==3">正式</span>
              </template>
            </el-table-column>
            <el-table-column prop="positionName" label="职位"/>
            <el-table-column prop="staffPhone" label="电话号码"/>
            <el-table-column prop="staffHiredate" label="入职日期"/>
            <el-table-column prop="workerDate" label="转正日期"/>
          </el-table>
        </div>

        <!-- 分页插件 -->
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
              @size-change="selectsocialStaffPage"
              @current-change="selectsocialStaffPage"
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
import qs from "qs";
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
      res2:"",
      // 部门  文本框的值
      dept:[],
      deptId:[],
      // 格式
      defaultProps,
      //存放部门信息
      deptlists: [],

      //选中的员工id
      xzStaffIds:[],
      // 多选时 数据
      tableVal:[],

      //按钮是否被禁用
      disableds:true,

      path: "/social/basic_setup/insured_scheme",
      // 参保方案
      scheme_name: null,
      // 参保方案下拉选择器值
      insured_scheme: [],
      name: "",
      // 分页参数
      pageInfo: {
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',
        //员工状态
        stateSearch:'',
      },
      // 部门名称
      dept_name: null,
      // 选择部门 下拉选择器
      depts: [
        {value: "1", label: "部门1"},
        {value: "2", label: "部门2"},
        {value: "3", label: "部门3"},
      ],
      // 表格上的 搜索框
      search: null,
      // 未参保人员表数据
      emps_table: [],
      // 表单验证
      rules: {
        name: [
          {required: true, message: '请输入活动名称', trigger: 'blur'},
          {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
        ],
      }
    };
  },
  methods:{

    // 表格多选框选中发生变化时调用的方法
    tableChange(val){

      this.tableVal=val

      // 清空选中 id
      this.xzStaffIds=[]

      // 循环获取选中行的id
      this.tableVal.forEach(item=>{
        this.xzStaffIds.push( item.staffId )
      })

      // 判断提交框是否可用
      this.sub()

    },

    // 判断提交框是否可用
    sub(){
      // 判断提交框是否可用
      if(this.xzStaffIds.length!=0 && this.scheme_name!=null && this.scheme_name!=""){
        this.disableds=false // 启用
      }else{
        this.disableds=true // 禁用
      }

    },

    // 提交，新增参保方案员工中间表
    submitAdd(){
      this.axios({
        url: 'http://localhost:8010/provider//socialStaffVo/insertsocial',
        method: 'post',
        data:{
          zbId:this.scheme_name,
          staffIds:this.xzStaffIds
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectEmps() // 修改完成后调用查询方法
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });


    },


    //搜索框重置
    replacement() {
      this.pageInfo.currentPage = 1,
          this.pageInfo.staffNameSearch = '',
          this.pageInfo.deptSearch = '',
          this.res2=""
          this.pageInfo.stateSearch='',
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys([], false)


      this.selectsocialStaffPage()

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

    //查询全部员工
    selectsocialStaffPage() {

      let params= {

        currenPage:this.pageInfo.currenPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,
        stateSearch: this.pageInfo.stateSearch,

      }

      this.axios
          .get("http://localhost:8010/provider/socialStaffVo/selectsocialPage?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(1111111111111);
            console.log(response);
            this.emps_table = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },


    // 查询所有参保方案
    selectAllPage() {
      this.axios
          .get("http://localhost:8010/provider/defInsured/selectAllPage",{params:{
              // 分页参数
              currentPage: 1, //当前页
              pagesize: 999, // 页大小
              input: "",// 参保方案名称搜索框的值
              state: 0,// 参保方案状态下拉框的值
            },})
          .then((response) => {
            console.log(response);
            this.insured_scheme =response.data.data.records
          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },
  created() {
    this.selectAllPage()
    this.selectDeptName()
    this.selectsocialStaffPage()
  }
};
</script>

<style scoped>

/*表格*/
.sub-Content__primary{
  margin-top:20px
}

/* 表单验证 */
::v-deep .el-form-item__error {
  margin-left: 135px;
}

/* 开关 */
.form-seitch {
  padding: 40px 0 40px 0;
  margin-left: -300px;
}

/* 表单左右div */
.form-div {
  margin: 10px;
  display: inline-block;
  border: 1px solid #e5e8ed;
  width: 400px;
  height: 300px;
}

/* 输入框 */
.el-input {
  width: 50%;
  display: inline-block;
}

/* 外div 内边距 */
.all-input {
  /*  上、右、下、左  */
  padding: 10px 40px 25px 40px;
}

/* 批量参保 */
.one {
  font-size: 18px;
  border-bottom: 1px solid #f4f5f7;
  padding-bottom: 5px;
}

.j-card-body {
  padding: 10px 25px 20px 25px;
}

/* 调整输入框的高度 */
/* ::v-deep .el-input__inner {
  height: 32px;
} */

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
  width: 120px;
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

/* 去掉路由样式 */
a {
  text-decoration: none;
  color: #4779b9;
}

@font-face {
  font-family: 'iconfont';  /* Project id 2990377 */
  src: url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff2?t=1639139822080') format('woff2'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.woff?t=1639139822080') format('woff'),
  url('//at.alicdn.com/t/font_2990377_1cwdx3n4atr.ttf?t=1639139822080') format('truetype');
}
</style>

