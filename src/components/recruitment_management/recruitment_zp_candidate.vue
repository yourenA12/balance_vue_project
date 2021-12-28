<!--&lt;!&ndash;简历：候选人&ndash;&gt;-->
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
          <!-- 批量导入按钮 -->
          &nbsp;
          <el-button size="small" type="warning" plain >
            <el-icon><i-download /></el-icon>
            批量导入
          </el-button>
          <!-- 批量批量删除按钮 -->
          <el-button size="small" type="danger" plain> <el-icon><i-delete /></el-icon>批量删除</el-button>
          <!-- 批量设为候选人 -->
          <el-button size="small" type="info"  plain >批量设为候选人</el-button>
<!--          <button style="margin-top: 4px; margin-left: 10px;" type="button" class="ant-btn abt" >-->
<!--            <span>批量设为候选人</span>-->
<!--          </button>-->


          <!--筛选框-->

          <!--搜索框-->
          <div style="float: right;">
            <el-form :inline="true" :model="formInline" class="demo-form-inline">

              <el-form-item>
                <el-input v-model="formInline.user" placeholder="姓名、招聘计划名称" clearable></el-input>
              </el-form-item>

              <el-form-item>
                <el-button type="primary" @click="" size="mini"><i class="iconfont">&#xeafe;</i></el-button>
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
<!--      <el-table :data="tableData" style="width: 100%; cursor: pointer" size="mini" :header-cell-style="{background:'#eef1f6',color:'#606266'}">-->
        <el-table :data="tableData" stripe style="width: 100%;" :header-cell-style="{background:'#eef1f6',color:'#606266'}" >
        <el-table-column fixed="left"  align="center" type="selection" width="80" />
        <el-table-column fixed="left" prop="name" label="姓名" width="150">
          <template #default="scope">
            <router-link :to="{path:this.details,query:{path:this.$route.query.path,name:scope.row.name}}">{{scope.row.name}}</router-link>
          </template>
        </el-table-column>
        <el-table-column fixed="left" prop="departm" label="投递部门" width="140"/>
        <el-table-column prop="gender" label="性别" width="140"/>
        <el-table-column prop="schoolli" label="学历" width="140"/>
        <el-table-column prop="phone" label="手机号" width="140"/>
        <el-table-column prop="age" label="年龄" width="140"/>
        <el-table-column prop="email" label="邮箱" width="140"/>
        <el-table-column prop="professional" label="专业" width="140"/>
        <el-table-column prop="birth" label="出生日期" width="140"/>
        <el-table-column prop="face" label="政治面貌" width="140"/>
        <el-table-column prop="gradschool" label="毕业学校" width="140"/>
        <el-table-column prop="invitation" label="是否邀约" width="140"/>
        <el-table-column prop="state" label="状态" width="140"/>

        <el-table-column fixed="right" label="操作" width="180">
          <template #default>
            <div style="width: 110px">
              <el-button type="text" size="small" @click="" style="color: #ea7c99;">删除</el-button>
              <el-button type="text" size="small" @click="">移出</el-button>
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
            @size-change="sele"
            @current-change="sele"
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

export default {
  data() {
    return {
      //路由地址
      details:'/recruitment/resume/details',
      addresume:'/recruitment/recruit/addresume',
      pageInfo: {
        currenPage: 1,
        /* 当前的页 */
        pagesize: 3,
        total: 0,
      },
      //筛选框显示隐藏
      icons:false,
      //搜索框
      input: "",
      //表格数据
      tableData: [
        {name:'tom',departm:'tom',gender:'tom',schoolli:'tom',phone:'tom',age:'tom',email:'tom',professional:'tom',birth:'tom',face:'tom',gradschool:'tom   ',invitation:'tom',state:'tom'},
        {name:'tom',departm:'tom',gender:'tom',schoolli:'tom',phone:'tom',age:'tom',email:'tom',professional:'tom',birth:'tom',face:'tom',gradschool:'tom   ',invitation:'tom',state:'tom'},
        {name:'tom',departm:'tom',gender:'tom',schoolli:'tom',phone:'tom',age:'tom',email:'tom',professional:'tom',birth:'tom',face:'tom',gradschool:'tom   ',invitation:'tom',state:'tom'}
      ],
      //筛选框数据
      formInline:{
        vlues1:'',
        vlues2:'',
        vlues3:'',
        vlues4:'',
        user:''

      }


    }
  },
  methods:{

  }

}

</script>
<style type="text/css" scoped>
/*@import url("../../css/navigation.css");*/
@import url("../../css/zpdaohang.css");
.demo-pagination-block{
  margin-left:850px ;
  margin-top: 20px;
  margin-bottom: 30px;
}

.icon-s{
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

/*.el-button--primary:hover {!* el-input 悬浮时 *!*/
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