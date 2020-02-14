<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img
          class="img_logo"
          src="../assets/MrJ-logo.png"
          alt=""
        >
        <span style="color:red">XXX电商后台管理系统</span>
      </div>
      <el-button
        type="info"
        @click="logout"
      >退出</el-button>
    </el-header>
    <!-- 页面主题区域 -->
    <el-container>
      <!-- 侧边栏区域 -->
      <el-aside :width="isCollpase ? '64px':'200px'">
        <div
          class="toggle-button"
          @click="toggleCollapse"
        >收起/展开</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#409eff"
          unique-opened
          :collapse="isCollpase"
          :collapse-transition="false"
          :router="true"
          :default-active='activePath'
        >
          <!-- 一级菜单 -->
          <el-submenu
            :index="item.id + ''"
            v-for="item in menulist"
            :key="item.id"
          >
            <!-- 模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/' + subItem.path)"
            >
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧主体区域 -->
      <el-container>
        <el-main>
          <router-view></router-view>
        </el-main>
        <!-- 底部区域 -->
        <el-footer>Footer</el-footer>
      </el-container>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollpase: false,
      // 被激活链接地址
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    // 点击按钮  切换折叠展开
    toggleCollapse() {
      this.isCollpase = !this.isCollpase
    },
    // 保存链接激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
    }
  }
}
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
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}
.el-main {
  background-color: #dee7f0;
}
.el-aside {
  background-color: #545c64;
  .el-menu {
    border-right: none;
  }
}
.el-footer {
  background-color: rgb(5, 20, 83);
}
.home-container {
  height: 100%;
}
.img_logo {
  position: relative;
  width: 100px;
  height: 60px;
}
.iconfont {
  padding: 10px;
}
.toggle-button {
  background-color: #4a5046;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
