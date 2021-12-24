<!--员工花名册页面-->
<template>
  <div class="saas-main-content">

    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body " >

        <!-- 菜单 -->

<!--          <div class="j-tabs2">-->
<!--            <el-menu-->
<!--                :default-active="activeIndex"-->
<!--                class="el-menu-demo"-->
<!--                mode="horizontal"-->
<!--                @select="handleSelect"-->
<!--            >-->
<!--              <router-link :to="{path:this.book,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
<!--                <el-menu-item style="height: 50px;" index="1">-->
<!--                  花名册-->
<!--                </el-menu-item>-->
<!--              </router-link>-->
<!--              <router-link :to="{path:this.business,query:{path: this.$route.query.path}}"-->
<!--                           style="text-decoration: none">-->
<!--                <el-menu-item style="height: 50px; " index="4"-->
<!--                >工作经历-->
<!--                </el-menu-item-->
<!--                >-->
<!--              </router-link>-->
<!--            </el-menu>-->
<!--          </div>-->

        <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
          <el-tab-pane label="花名册">
            <template #label>
              <router-link :to="{path:this.book,query:{path: this.$route.query.path}}" style="text-decoration: none">花名册&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>
            <router-view></router-view>

          </el-tab-pane>
          <el-tab-pane label="工作经历">
            <template #label>
              <router-link :to="{path:this.business,query:{path: this.$route.query.path}}" style="text-decoration: none">&nbsp;&nbsp;&nbsp;工作经历&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>

            <router-view></router-view>
          </el-tab-pane>


          <el-tab-pane label="奖罚记录&nbsp;&nbsp;&nbsp;&nbsp;">角色管理</el-tab-pane>
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
      business: '/employee/message/employee_roster/business'
    }
  },
  methods: {
    // //菜单点击事件
    // handleAsideSelect(index, key, keyPath) {
    //   //默认激活的路由
    //   this.$store.state.default_route = index
    //   //更换默认二级路由名称版本
    //   this.$store.state.router_versions = this.$route.query.path
    //   //跳转路由 通过path 以及携带参数  参数是从一级目录传过来的
    //   this.$router.push({
    //     path: index,
    //     query: {path: this.$route.query.path}
    //   })
    // },

  },

  mounted(){
    // 页面进入前调用方法
  }

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
