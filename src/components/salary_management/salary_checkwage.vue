<!--查看固定工资导航栏-->
<template>
  <div class="saas-main-content">

    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body " >

        <!--         菜单 -->

        <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
          <el-tab-pane label="固定工资">
            <template #label>
              <router-link :to="{path:this.fixedsalary,query:{path: this.$route.query.path}}" style="text-decoration: none">固定工资&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>
            <router-view></router-view>

          </el-tab-pane>
          <el-tab-pane label="调薪查询">
            <template #label>
              <router-link :to="{path:this.raise,query:{path: this.$route.query.path}}" style="text-decoration: none">&nbsp;&nbsp;&nbsp;调薪查询&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>

            <router-view></router-view>
          </el-tab-pane>

        </el-tabs>

      </div>
    </div>
  </div>

  &nbsp;
</template>

<script>
export default {
  data() {
    return {
      fixedsalary: '/salary/fixedsalary',
      raise: '/salary/raise',

    }
  },


}
</script>


<style scoped>
@import url("../../css/navigation.css");

/deep/ .mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

</style>
