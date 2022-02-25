<!--简历：全部简历-->
<template>
  <div class="sub-Content__primary">
    <div class="ant-spin-nested-loading">
      <div class="ant-spin-container">
        <div class="mt-20 ml-20 mr-20">
          <!-- 新增招聘计划按钮 -->
          <a style="margin-top: 4px;">
            <router-link :to="{path:this.addresume,query:{path:this.$route.query.path}}">
              <el-button size="small" type="primary" plain>
                <el-icon><i-plus/></el-icon>
                新增
              </el-button>
            </router-link>
          </a>

<!--          &lt;!&ndash;          批量删除&ndash;&gt;-->
<!--          &nbsp;-->
<!--          <el-button size="small" type="danger" plain>-->
<!--            <el-icon><i-delete /></el-icon>-->
<!--            批量删除-->
<!--          </el-button>-->

<!--          &lt;!&ndash; 批量导入按钮 &ndash;&gt;-->

<!--          <el-button  size="small" type="warning" plain >-->
<!--            <el-icon><i-download /></el-icon>-->
<!--            批量导入-->
<!--          </el-button>-->

<!--          &lt;!&ndash; 批量设为候选人 &ndash;&gt;-->
<!--          <el-button size="small" type="info"  plain >批量设为候选人</el-button>-->


          <!--筛选框-->

          <!--搜索框-->
          <div style="float: right;">
            <el-form :inline="true" :model="pageInfo" class="demo-form-inline">

              <el-form-item>
                <el-input v-model="pageInfo.resumeName" placeholder="姓名、招聘计划名称" clearable></el-input>
              </el-form-item>

              <el-form-item>
                <el-button type="primary" @click="selectresume_plan()" size="mini"><i class="iconfont">&#xeafe;</i></el-button>
              </el-form-item>
            </el-form>
          </div>

        </div>
      </div>
      <!--
        筛选框
      -->

    </div>
    <br/>
    <!-- 表格数据 -->
    <div class="ant-table-wrapper j_statistics_layout">
<!--      <el-table :data="tableData" style="width: 100%; cursor: pointer" size="mini"-->
<!--                :header-cell-style="{background:'#eef1f6',color:'#606266'}">-->
        <el-table :data="tableData" stripe style="width: 100%;" :header-cell-style="{background:'#eef1f6',color:'#606266'}" >
        <el-table-column fixed="left" align="center" type="selection" width="80"/>
        <el-table-column fixed="left" prop="resumeName" label="姓名" width="120">
          <template #default="scope">
<!--            <router-link :to="{path:this.details,query:{path:this.$route.query.path,resumeName:scope.row.resumeName}}">{{scope.row.resumeName}}</router-link>-->
<!--            <router-link :to="{path:this.details,query:{path:this.$route.query.path,resumeName:scope.row.resumeName}}">-->
            <el-button type="text" size="small" @click="empMsg(scope.row.resumeId)"
            >{{scope.row.resumeName}}
            </el-button>
<!--            </router-link>-->
          </template>
        </el-table-column>
        <el-table-column fixed="left" prop="deptName" label="招聘部门" width="140"/>
        <el-table-column prop="resumeSex" label="性别" width="140"/>
        <el-table-column prop="resumeEducation" label="学历" width="140"/>
        <el-table-column prop="resumePhone" label="手机号" width="140"/>
          <el-table-column prop="resumeAge" label="年龄" width="140"/>
        <el-table-column prop="resumeMailbox" label="邮箱" width="140"/>
<!--        <el-table-column prop="educationMajor" label="专业" width="140"/>-->
        <el-table-column prop="resumeBirthday" label="出生日期" width="140"/>
        <el-table-column prop="resumePoliticalOutlook" label="政治面貌" width="140"/>
        <el-table-column prop="resumeScoller" label="毕业学校" width="140"/>
<!--        <el-table-column prop="inviteState" label="是否邀约" width="140"/>-->
<!--          <el-table-column label="是否邀约" width="140">-->
<!--            <template #default="scope">-->
<!--              <span v-if="scope.row.inviteState==1">已邀约</span>-->
<!--              <span v-if="scope.row.inviteState==0">未邀约</span>-->
<!--            </template>-->
<!--          </el-table-column>-->
          <el-table-column label="状态" width="140">
            <template #default="scope">
              <span v-if="scope.row.resumeZt==0">新简历</span>
              <span v-if="scope.row.resumeZt==1">候选人</span>
              <span v-if="scope.row.resumeZt==2">已邀约</span>
              <span v-if="scope.row.resumeZt==3">已淘汰</span>
              <span v-if="scope.row.resumeZt==4">面试中</span>
              <span v-if="scope.row.resumeZt==5">面试通过</span>
              <span v-if="scope.row.resumeZt==6">复试中</span>
              <span v-if="scope.row.resumeZt==7">待接受</span>
              <span v-if="scope.row.resumeZt==8">已接受</span>
              <span v-if="scope.row.resumeZt==9">已入职</span>
              <span v-if="scope.row.resumeZt==10">已拒绝</span>
              <span v-if="scope.row.resumeZt==11">放弃入职</span>
              <span v-if="scope.row.resumeZt==12">已离职</span>
            </template>
          </el-table-column>



          <el-table-column fixed="right" label="操作" width="180">
          <template #default="scope">
            <div style="width: 110px">
              <el-button type="text" size="small" @click="">设为候选人</el-button>

<!--              <el-button type="text" size="small" @click="open()" style="color: #ea7c99;">删除</el-button>-->
              <el-popconfirm title="是否确认删除?" @confirm="confirmsc(scope.row)" @cancel="cancelsc()">
                <template #reference>
                  <el-button type="text" size="small" style="color: #f10c36;">删除</el-button>
                </template>
              </el-popconfirm>

            </div>

          </template>
        </el-table-column>
      </el-table>

      <div class="demo-pagination-block">
        <!-- <span class="demonstration">All combined</span> -->
        <el-pagination
            v-model:currentPage="pageInfo.currenPage"
            :page-sizes="[3, 5, 10, 50]"
            v-model:page-size="pageInfo.pagesize"
            :default-page-size="pageInfo.pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="pageInfo.total"
            :pager-count="5"
            background
            @size-change="selectresume_plan"
            @current-change="selectresume_plan"
        >
        </el-pagination>
      </div>
    </div>


  </div>

</template>

<script>
import {
  ref
} from 'vue'
import {ElMessageBox, ElMessage} from 'element-plus'

export default {
  data() {

    return {
      //路由地址
      addresume:'/recruitment/recruit/addresume',
      details:'/recruitment/resume/details',
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
        //名称
        resumeName:"",
      },
      fo:{
        resumeId:"",
        resumeZt:3,
      },

      //筛选框显示隐藏
      icons: false,
      //搜索框
      input: "",
      //表格数据
      tableData: [

      ],
      //筛选框数据
      formInline: {
        vlues1: '',
        vlues2: '',
        vlues3: '',
        vlues4: '',
        user: ''
      }
    }
  },
  methods: {
    //删除按钮消息提示框 事件
    open() {
      ElMessageBox.confirm(
          '是否删除该数据?',
          {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
          })
          .then(() => {
            //删除事件
            console.log("123")
            ElMessage({
              type: 'success',
              message: '删除成功',
            })
          })
          .catch(() => {
            ElMessage({
              type: 'info',
              message: '已取消操作',
            })
          })
    },
    //消息提示框确认按钮事件
    confirmgb(){
      ElMessage({
        message: '操作成功',
        type: 'success',
      })
    },
    //消息提示框取消按钮事件
    cancelgb(){
      ElMessage({
        message: '已取消该操作',
        type: 'warning',
      })
    },
    //消息提示框确认按钮事件
    confirmsc(row){

      this.Ztxiugai(row)
      // ElMessage({
      //   message: '操作成功',
      //   type: 'success',
      // })
    },
    //消息提示框取消按钮事件
    cancelsc(){
      ElMessage({
        message: '已取消该操作',
        type: 'warning',
      })
    },

    //查询
  selectresume_plan(){
    this.axios
        .get("http://localhost:8010/provider/ResumeVo/ResumePage/",{params:this.pageInfo})
        .then((response) => {
          console.log(response);
          this.tableData = response.data.data.records;
          // console.log(response.data.data.records);
          this.pageInfo.total = response.data.data.total;
        })
        .catch(function (error){
          console.log(error);
        })
  },
    //修改
    Ztxiugai(row){

      this.fo.resumeId=row.resumeId

      this.axios
          .put("http://localhost:8010/provider/resume/resume/zeliminate",this.fo)
          .then((response) => {

            if( response.data.data ==="成功" ){
              ElMessage.success('修改成功')
              this.selectresume_plan()
            }else{
              ElMessage.error('修改失败')
            }
          }).catch(function (error){
        console.log(error);
      })
    },
    empMsg(resumeId){
      this.$router.push({path:this.details,query:{path:this.$route.query.path,resumeId:resumeId}})
    }

},
  created(){
    this.selectresume_plan();
  }
}
</script>
<style type="text/css" scoped>
/*@import url("../../css/navigation.css");*/
@import url("../../css/zpdaohang.css");

.demo-pagination-block {
  margin-left: 850px;
  margin-top: 20px;
  margin-bottom: 30px;
}

.icon-s {
  width: 75%;
  height: 45px;
  border-radius: 4px;
  border: 1px solid #ddd;
  margin-top: 10px;
  padding: 16px;
  margin-left: 70px;
}

/*.el-button--primary { !* el-input 显示时 *!*/
/*  background: #085fc3 !important;*/
/*  border-color: #085fc3 !important;*/
/*}*/

/*.el-button--primary:hover { !* el-input 悬浮时 *!*/
/*  background: #297ccf !important;*/
/*  border-color: #297ccf !important;*/
/*  color: #FFF !important;*/
/*}*/

.j-tabs {
  border-bottom: 1px solid #d9d9d9;
}

.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

.ant-spin-nested-loading {
  position: relative;
}

.ant-spin-container {
  position: relative;
  transition: opacity 0.3s;
}

.ant-table-wrapper::after {
  clear: both;
}

.ant-table-wrapper::before, .ant-table-wrapper::after {
  display: table;
  content: '';
}

.abt:hover {
  color: #0c9c6e;
  border: 1px solid #0c9c6e;
}
</style>

