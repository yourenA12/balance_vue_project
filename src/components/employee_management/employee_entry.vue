<!--入职管理页面-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <!-- 菜单 -->

        <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
          <el-tab-pane label="待入职员工">
            <template #label>
              <router-link :to="{path:this.tobehired,query:{path: this.$route.query.path}}" style="text-decoration: none">待入职员工&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
            </template>
            <router-view></router-view>

          </el-tab-pane>
          <el-tab-pane label="放弃入职员工">
            <template #label>
              <router-link :to="{path:this.hashired,query:{path: this.$route.query.path}}" style="text-decoration: none">&nbsp;&nbsp;&nbsp;放弃入职员工&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</router-link>
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
  data(){
    return{
      tobehired:'/employee/transaction/entry/tobehired',
      hashired:'/employee/transaction/entry/hashired',

    }
  },

}
</script>


<style scoped>
@import url("../../css/navigation.css");
/deep/.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}

.active{
  border-bottom: 1px solid #085fc3;
}
.sactive{
  color: #085fc3;
}


</style>

