<!--员工花名册页面-->
<template>
  <div class="saas-main-content">

    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body " >

<!--         菜单 -->

        <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
          <el-tab-pane label="花名册">
            <template #label>
              <router-link :to="{path:this.book,query:{path: this.$route.query.path}}" style="text-decoration: none">花名册&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>
            <router-view></router-view>

          </el-tab-pane>

<!--          <el-tab-pane label="工作经历">-->
<!--            <template #label>-->
<!--              <router-link :to="{path:this.business,query:{path: this.$route.query.path}}" style="text-decoration: none">&nbsp;&nbsp;&nbsp;工作经历&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>-->
<!--            </template>-->

<!--            <router-view></router-view>-->
<!--          </el-tab-pane>-->

          <el-tab-pane label="奖罚记录">
            <template #label>
              <router-link :to="{path:this.record,query:{path: this.$route.query.path}}" style="text-decoration: none">&nbsp;&nbsp;&nbsp;奖罚记录&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
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
      book: '/employee/message/employee_roster/book',
      business: '/employee/message/employee_roster/business',
      record:'/employee/message/employee_roster/record'
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
