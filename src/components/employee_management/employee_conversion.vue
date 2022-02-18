<!--转正管理-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <br/>
        <!--搜索输入框-->

<!--        <div style="width:100%;height: 100px;margin: auto;padding: 12px 24px;">-->
<!--          <div class="staff_div_2" style="float:left;">-->

<!--            <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe61f;</i>-->
<!--            <div class="staff_div_2_text">-->
<!--              <span>实习期人员</span><br/><span>2</span>-->
<!--            </div>-->
<!--          </div>-->

<!--          <div class="staff_div_2 " style="float: right">-->
<!--            <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe617;</i>-->
<!--            <div class="staff_div_2_text">-->
<!--              <span>转正已生效</span><br/><span>2</span>-->
<!--            </div>-->
<!--          </div>-->
<!--        </div>-->

        <div style="margin-top:10px;">
          <!--搜索输入框-->
          <div style="display: inline-block">
            <span style="font-weight:bold;margin-left: 30px">员工名称</span>
            <el-row style="width:200px;display: inline-block;margin-left: 15px">
              <el-input v-model="pageInfo.staffNameSearch" placeholder="请输入用户名称"/>
            </el-row>
          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold">部门 </span>

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


          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold">职位 </span>

            <el-select v-model="pageInfo.postSearch" placeholder="请输入部门名称" style="width: 200px;margin-left: 15px">
              <el-option
                  v-for="item in positionAll"
                  :key="item.positionId"
                  :label="item.positionName"
                  :value="item.positionId"
              >
              </el-option>
            </el-select>

          </div>

          <div style="display: inline-block;margin-left:25px">
            <span style="font-weight:bold"> 快转正员工</span>

            <el-select placeholder="请选择状态" v-model="pageInfo.stateSearch" style="margin-left: 15px;">
              <el-option label="近一周" value="7" style="margin-left: 15px"></el-option>
              <el-option label="近一个月" value="30" style="margin-left: 15px"></el-option>
            </el-select>

          </div>
          <div style="display: inline-block;margin-left:25px;margin-top:20px">

            <div class="block">
              <span class="demonstration" style="font-weight:bold">入职日期</span>
              <el-date-picker style="margin-left: 15px;"
                              v-model="hiredateSearch"
                              type="daterange"
                              unlink-panels
                              range-separator="至"
                              start-placeholder="开始时间"
                              end-placeholder="结束时间"
                              :shortcuts="shortcuts"
                              value-format="YYYY-MM-DD"
              >
              </el-date-picker>

            </div>
          </div>


          <el-button @click="selectProbation()" type="primary" style="width: 70px;margin-left:25px">
            <el-icon>
              <i-search/>
            </el-icon>
            搜索
          </el-button>
          <el-button style="width: 70px;" @click="replacement()">
            <el-icon>
              <i-refresh/>
            </el-icon>
            重置
          </el-button>
        </div>
        <br/>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="tableData"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column prop="staffName" label="姓名" width="180"/>
            <el-table-column prop="staffIdentity" label="证件号码" width="180"/>
            <el-table-column prop="deptName" label="部门" width="180"/>
            <el-table-column prop="positionName" label="职位" width="180"/>
            <el-table-column prop="staffHiredate" label="入职日期" width="180"/>
            <el-table-column label="试用期限" width="180">
              <template #default>
                3个月
              </template>
            </el-table-column>

            <el-table-column fixed="right" label="操作">
              <template #default="scope">
                <el-button type="text" size="small" @click="positive(scope.row)">办理转正</el-button>
              </template>
            </el-table-column>
          </el-table>
        </div>


        <!-- 分页插件 -->
        <div class="demo-pagination-block">
          <el-pagination
              v-model:currenPage="pageInfo.currentPage"
              :page-sizes="[3, 5, 10, 50]"
              v-model:page-size="pageInfo.pagesize"
              :default-page-size="pageInfo.pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pageInfo.total"
              :pager-count="5"
              background
              @size-change="selectProbation"
              @current-change="selectProbation"
          >
          </el-pagination>
        </div>


      </div>
    </div>
  </div>
  <div>
    <el-dialog
        v-model="become"
        title="转正"
        width="50%"
        :close-on-click-modal="false"
    >
      <el-form
          ref="form_1"
          :model="positiveTK"
          label-width="120px"
          :rules="rules"
      >
        <div style="width:100%;margin-left: -20px;">
          <div style="display: inline-block;width:50%;">
            <el-form-item label="员工名称 :" prop="staffName" style="">
              <el-input v-model="positiveTK.staffName" disabled style="width:240px;"></el-input>
            </el-form-item>

            <el-form-item label="职位 :">
              <el-input v-model="positiveTK.postName" disabled style="width:240px;"></el-input>
            </el-form-item>

            <el-form-item label="试用期限 :">
              <el-input v-model="positiveTK.TrialPeriod" disabled style="width:240px;"></el-input>
            </el-form-item>

            <el-form-item label="原转正日期:" prop="workerDate">
              <el-date-picker
                  v-model="positiveTK.workerDate"
                  type="date"
                  placeholder="选择时间" style="width:240px;" disabled
              >
              </el-date-picker>
            </el-form-item>

            <el-form-item label="转正备注 :">
              <el-input
                  v-model="positiveTK.remarks"
                  type="textarea"
                  maxlength="500"
                  show-word-limit
                  prop="remarks_1" style="width:240px;"
              ></el-input>
            </el-form-item>


          </div>
        <div style="display: inline-block;width:50%;position: relative;top:-60px">
        <el-form-item label="部门名称 :" prop="dept">
          <el-input v-model="positiveTK.deptName" disabled style="width:240px;"></el-input>
        </el-form-item>

        <el-form-item label="入职日期 :">
          <el-input v-model="positiveTK.staffHiredate" disabled style="width:240px;"></el-input>
        </el-form-item>

        <el-form-item label="转正类型" prop="workerType" style="width:600px">
          <el-select
              v-model="positiveTK.workerType"
              placeholder="请选择" style="width:240px;"
          >
            <el-option label="转正" value="转正" ></el-option>
            <el-option label="提前转正" value="提前转正" ></el-option>
            <el-option label="延迟转正" value="延迟转正" ></el-option>
          </el-select>
        </el-form-item>

          <el-form-item label="转正日期 :" prop="workersDates">
            <el-date-picker
                v-model="positiveTK.workersDates"
                type="date"
                placeholder="选择时间" style="width:240px;"
            >
            </el-date-picker>
          </el-form-item>


        </div>
          <div style="margin-left: 310px;margin-top: 30px">
            <el-button  @click="become=false" style="width:80px">取消</el-button>
            <el-button type="primary" style="width:80px" @click="insertWorker_Staff()">确定</el-button>

          </div>
        </div>

      </el-form>

    </el-dialog>


  </div>
</template>

<script >
import {defineComponent, ref} from 'vue'
import {ElMessage} from "element-plus/es";
import qs from "qs";
export default defineComponent({

  data() {
    const one = (rule, value, callback) => {
      if (new Date() > value) {
        callback(new Error("转正日期不能小于当前时间"));
      } else {
        callback();
      }
    };

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


      // 入职日期 时间段
      hiredateSearch: [],
      //存储部门名称
      deptNameAll:[],
      //存储职位名称
      positionAll:[],
      //时间选择
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
      tableData: [],
      seek: "",
      positiveTK: {
        //员工编号
        staffId:'',
        //部门编号
        deptId:'',
        //员工姓名
        staffName: '',
        //部门
        deptName: '',
        //职位
        postName: '',
        //入职日职
        staffHiredate: '',
        //试用期限
        TrialPeriod:'3个月',
        //转正类型
        workerType:'',
        //原定计划转正日期
        workerDate:'',
        //转正日期
        workersDates:'',
        //备注
        workerRemarks:'',
      },
      pageInfo: {
        // 分页参数
        currenPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数

        //搜索绑定值
        // 员工名称
        staffNameSearch: '',
        // 部门名称
        deptSearch: '',
        //部门职位名称
        postSearch:'',
        //快转正员工
        stateSearch:'',

        // 入职日期  开始时间
        clockTimeStart: '',
        // 结束时间
        clockTimeEnd: ''
      },
      rules: {
        type: [
          {
            required: true,
            message: '请选择转正类型',
            trigger: 'change',
          },
        ],
        becomedate: [
          {
            required: true,
            message: '请选择转正日期',
            trigger: 'change'
          },
          {
            validator: one, trigger: "change"
          },
        ],
        workerDate: [
          {
            required: true,
            message: '请选择转正日期',
            trigger: 'change'
          },
          {
            validator: one, trigger: "change"
          },
        ],
      },
      //转正
      workerVal:null,
      staffVal:null,
    };
  },
  setup() {
    const become = ref(false)
    return {
      become,
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    //搜索框重置
    replacement() {
      this.pageInfo.currentPage = 1,
          this.pageInfo.staffNameSearch = '',
          this.res2=""
      // 将值赋值到选择器中
      this.$refs.tree.setCheckedKeys([], false)
      this.pageInfo.postSearch = '',
          this.pageInfo.stateSearch = '',
          this.pageInfo.clockTimeStart = '',
          this.pageInfo.clockTimeEnd = ''

          this.selectProbation()

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

    //查询试用期人员
    selectProbation() {

      // 首先清空
      this.pageInfo.clockTimeStart = ""  // 开始时间
      this.pageInfo.clockTimeEnd = "" // 结束时间
      if (this.hiredateSearch != "") { // 如果选择的打卡时间不为空
        this.pageInfo.clockTimeStart = this.hiredateSearch[0] // 取 入职日期选择框 的开始时间 就是数组下标为0（第一个）
        this.pageInfo.clockTimeEnd = this.hiredateSearch[1]
      }

      let params= {

        currenPage:this.pageInfo.currenPage,
        pagesize:this.pageInfo.pagesize,
        staffNameSearch: this.pageInfo.staffNameSearch,
        deptIds:this.res2.length==0?'':this.res2,
        postSearch:this.pageInfo.postSearch,
        stateSearch:this.pageInfo.stateSearch,
        clockTimeStart:this.pageInfo.clockTimeStart,
        clockTimeEnd:this.pageInfo.clockTimeEnd,

      }

      this.axios
          .get("http://localhost:8010/provider/staff/selectProbation?"+qs.stringify(params,{ arrayFormat: 'repeat' }))
          .then((response) => {
            console.log(response);
            this.tableData = response.data.data.records;
            console.log(response.data.data.records)
            this.pageInfo.total = response.data.data.total;
          })
          .catch(function (error) {
            console.log(error);
          });
    },
    //获取转正管理数据 赋在弹出框上
    positive(row){
      this.become=true;
      //员工编号
      this.positiveTK.staffId=row.staffId,
      //部门编号
      this.positiveTK.deptId=row.deptId,
      //员工姓名
      this.positiveTK.staffName=row.staffName,
          //部门
      this.positiveTK.deptName=row.deptName,
          //职位
      this.positiveTK.postName=row.postName,
          //入职日职
      this.positiveTK.staffHiredate=row.staffHiredate,
          //原定计划转正日期
      this.positiveTK.workerDate=row.workerDate

    },
    //添加转正表和修改员工信息
    insertWorker_Staff(){
      //获取弹出框上的值加入到转正表
      this.workerVal={
        //员工编号
       staffId:this.positiveTK.staffId,
        //员工名称
        staffName:this.positiveTK.staffName,
        //部门编号
        deptId:this.positiveTK.deptId,
        //转正类型
        workerType:this.positiveTK.workerType,
        //备注
        workerRemarks:this.positiveTK.workerRemarks,
        //状态
        workerState:1,
        //转正时间
        workerDate:this.positiveTK.workersDates
      }

      //修改员工表状态和转正日期
      this.staffVal={
        //员工编号
        staffId:this.positiveTK.staffId,
        //转正时间
        workerDate:this.positiveTK.workersDates,
        //员工状态
        staffState:3
      }
      //调用添加转正
      this.insertWorker()

    },
    //添加转正表
    insertWorker(){
      this.axios({
        url: 'http://localhost:8010/provider/worker/insertWorker',
        method: 'post',
        data:{
          Worker:this.workerVal,
          Staff:this.staffVal,
        }
      }).then(response => {
        if (response.data.data > 0) {
          ElMessage({
            message: '添加成功',
            type: 'success',
          })
          this.selectProbation() // 修改完成后调用查询方法
          this.become=false
        } else {
          ElMessage.error('添加失败')
        }
      }).catch(function (error) {
        console.log(error);
      });

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
    //查询职位名称
    selectPositionName() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectPositionName")
          .then((response) => {
            console.log(response);
            this.positionAll = response.data.data;

          })
          .catch(function (error) {
            console.log(error);
          });
    },
  },created() {
    this.selectProbation();
    this.selectDeptName();
    this.selectPositionName();
  },
})
</script>


<style scoped>
@import url("../../css/navigation.css");

/deep/ .mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

/deep/ .cell {
  padding-left: 10px;
  text-align: center;
  color: black;
}

/deep/ .el-form-item {
  display: flex;
  margin-bottom: 22px;
}

/deep/ .el-card__body {
  /* padding: var(--el-card-padding); */
  padding: 10px 0;
}


/deep/ .icon-p p {
  text-align: center;
}

/* 分页的样式 */
/deep/ .demo-pagination-block {
  float: right;
  margin: 20px;
}

.staff_div_2_text {
  position: absolute;
  top: 20px;
  left: 529px;
  text-align: right;
  font-weight: bold;
  color: #085fc3;
}


.staff_div_2 {
  width: 49%;
  height: 80px;
  border: 1px solid #f0f0f0;
  display: inline-block;
  border-radius: 5px;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.2);
  position: relative;
}

</style>
