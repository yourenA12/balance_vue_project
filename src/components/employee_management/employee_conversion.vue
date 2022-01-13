<!--转正管理-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <br/>
        <!--搜索输入框-->
        <el-row style="width: 200px; margin-left: 1110px">
          <el-input v-model="seek" placeholder="搜索">
            <template #suffix>
              <el-icon class="el-input__icon">
                <i-search/>
              </el-icon>
            </template>
          </el-input>
        </el-row>
        <br/>
        <div style="width:100%;height: 100px;margin: auto;padding: 12px 24px;">
          <div class="staff_div_2" style="float:left;">

            <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe61f;</i>
            <div class="staff_div_2_text">
              <span>实习期人员</span><br/><span>2</span>
            </div>
          </div>

          <div class="staff_div_2 " style="float: right">
            <i class="iconfont" style="position: absolute;left:20px;top:10px;">&#xe617;</i>
            <div class="staff_div_2_text">
              <span>转正已生效</span><br/><span>2</span>
            </div>
          </div>
        </div>

        <!-- 表格内容部分 -->
        <div class="sub-Content__primary">
          <el-table :data="tableData"
                    :header-cell-style="{textAlign: 'center',background:'#f8f8f9',color:'#6C6C6C'}"
                    :cell-style="{textAlign: 'center'}">
            <el-table-column prop="staffName" label="姓名" width="180"/>
            <el-table-column prop="staffIdentity" label="证件号码" width="180"/>
            <el-table-column prop="deptName" label="部门" width="180"/>
            <el-table-column prop="postName" label="职位" width="180"/>
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
              v-model:currentPage="pageInfo.currentPage"
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
            <el-option label="转正" value="转正" style="margin-left: 20px"></el-option>
            <el-option label="提前转正" value="提前转正" style="margin-left: 20px"></el-option>
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

<script lang="ts">
import {defineComponent, ref} from 'vue'
import {ElMessage} from "element-plus/es";

export default defineComponent({

  data() {
    const one = (rule, value, callback) => {
      if (new Date() > value) {
        callback(new Error("转正日期不能小于当前时间"));
      } else {
        callback();
      }
    };
    return {
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
        currentPage: 1, //当前页
        pagesize: 3, // 页大小
        total: 0, // 总页数
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
    // RestForm() {
    //   this.become_1 = {
    //     name: '',
    //     dept: '',
    //     post: '',
    //     entrydate: '',
    //     tryoutdate: '',
    //     type: '',
    //     remarks: '',
    //     becomedate: ''
    //   }
    // },
    //查询试用期人员
    selectProbation() {
      this.axios
          .get("http://localhost:8010/provider/staff/selectProbation/"+this.pageInfo.currentPage+"/"+this.pageInfo.pagesize)
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
  },created() {
    this.selectProbation();
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
