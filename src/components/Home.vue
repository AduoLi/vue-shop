<template>
  <el-container class="home-ct">
    <el-header>
      <div class>
        <img src="../assets/head.jpg" alt srcset />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="out">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isShow?'64px':'200px'">
        <div class="toggle-menu" @click="toggleCollapse">|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409bff"
          unique-opened
		  :collapse="isShow"
		  :collapse-transition = "false"
		  router
        >
          <el-submenu :index="item.id + ''" v-for="(item, index) in menusList" :key="item.id">
            <template slot="title">
              <i :class="iconList[index]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item
              :index="'/' + item1.path"
              v-for="item1 in item.children"
              :key="item1.id"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{item1.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
		  <router-view></router-view>
	  </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menusList: [],
      iconList: [
        "iconfont icon-user",
        "iconfont icon-tijikongjian",
        "iconfont icon-shangpin",
        "iconfont icon-danju",
        "iconfont icon-baobiao",
	  ],
	  isShow:false
    };
  },
  created() {
    this.getMenusList();
  },
  methods: {
    out() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    getMenusList() {
      this.$http.get("menus").then((res) => {
        if (res.data.meta.status != 200)
          return this.$message.error(res.data.meta.msg);
        this.menusList = res.data.data;
        console.log(this.menusList);
      });
	},
	toggleCollapse() {
		this.isShow = !this.isShow
	}
  },
};
</script>

<style lang="less" scoped>
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin-right: 20px;
  }
  div {
    display: flex;
    align-items: center;
  }
}
.el-aside {
  background-color: #333744;
  transition: all 0.3s;
  .iconfont {
    margin-right: 10px;
  }
  .el-menu {
    border-right: none;
  }
  .toggle-menu {
    background-color: #4a5064;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 25px;
    color: #fff;
    cursor: pointer;
    font-size: 10px;
    letter-spacing: 0.2em;
  }
}
.el-main {
  background-color: #eaedf1;
}
.home-ct {
  width: 100%;
  height: 100%;
}
</style>