<!--入职管理页面-->
<template>
  <div class="saas-main-content">
    <div class="j-card j-card-bordered mainContent">
      <div class="j-card-body">
        <span></span>
        <!-- 菜单 -->

<!--          <div class="j-tabs2">-->
<!--            <el-menu-->
<!--              :default-active="activeIndex"-->
<!--              class="el-menu-demo"-->
<!--              mode="horizontal"-->
<!--              @select="handleSelect"-->
<!--            >-->
<!--              <router-link :to="{path:this.tobehired,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
<!--              <el-menu-item style="height: 50px; " index="1"-->
<!--                >待入职员工</el-menu-item-->
<!--              >-->
<!--            </router-link>-->
<!--              <router-link :to="{path:this.hashired,query:{path: this.$route.query.path}}" style="text-decoration: none">-->
<!--              <el-menu-item style="height: 50px; " index="4"-->
<!--                >放弃入职员工</el-menu-item-->
<!--              >-->
<!--            </router-link>-->
<!--            </el-menu>-->
<!--          </div>-->

<!--        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">-->
<!--          <el-tab-pane label="待入职员工" name="first">-->
<!--            <template #label>-->
<!--              <router-link :to="{path:this.tobehired,query:{path: this.$route.query.path}}" style="text-decoration: none">待入职员工</router-link>-->
<!--            </template>-->
<!--          </el-tab-pane>-->

<!--          <el-tab-pane label="放弃入职员工" name="second">-->
<!--            <template #label>-->
<!--              <router-link :to="{path:this.hashired,query:{path: this.$route.query.path}}" style="text-decoration: none">放弃入职员工</router-link>-->
<!--            </template>-->
<!--          </el-tab-pane>-->

<!--        </el-tabs>-->


        <div class="j-tabs">
          <ul>

            <li @click="liback(1)">
              <div v-bind:class="{active:isActive1}">

                <router-link :to="{path:this.tobehired,query:{path: this.$route.query.path}}" style="text-decoration: none">待入职员工</router-link>
              </div>
            </li>



            <li @click="liback(2)">
              <div v-bind:class="{active:isActive2}">
                <router-link :to="{path:this.hashired,query:{path: this.$route.query.path}}" style="text-decoration: none">放弃入职员工</router-link>
              </div>
            </li>


          </ul>
        </div>


        <section
            class="ant-layout ant-layout-has-sider"
            style="
              min-width: 988px;
              min-height: 90vh;
              background-color: rgba(232, 239, 246, 0);
              box-shadow: rgb(121, 159, 197) 0px 7px;
            "
            id="scrollLayout"
          >
            <main style="margin:10px" class="ant-layout-content">
              <router-view></router-view>
            </main>
          </section>
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

      isActive1:true,
      isActive2:false,

    }
  },
  methods:{
    liback(n) {
      if (n === 1) {
        this.isActive1 = true;
        this.isActive2 = false;
        this.isActive3 = false;
        this.isActive4 = false;
      }
      if (n === 2) {
        this.isActive1 = false;
        this.isActive2 = true;
        this.isActive3 = false;
        this.isActive4 = false;
      }
    }
    }
}
</script>


<style scoped>
@import url("../../css/navigation.css");
/deep/.mainContent .sub-Content__primary {
  padding: 12px 24px;
  background: #fff;
  border-radius: 4px;
}
.j-tabs {
  border-bottom: 1px solid #d9d9d9;
}
.j-tabs ul {
  margin-bottom: -2px;
  padding-left: 0px;
}
.j-tabs li {
  margin: 0;
  display: inline-block;
  -webkit-transition: all 0.3s ease;
  transition: all 0.3s ease;
  cursor: pointer;
  border: 0;
}
.j-tabs li a {
  display: inline-block;
  height: 54px;
  line-height: 54px;
  color: #000;
  padding: 0 21px;
  font-size: 14px;
}
.active{
  border-bottom: 1px solid #085fc3;
}
.sactive{
  color: #085fc3;
}


</style>

