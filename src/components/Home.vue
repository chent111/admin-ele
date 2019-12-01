<!--  -->
<template>
  <el-container class="home_container">
    <el-aside :width="isColleapsed ? '60px' : '200px'">
      <!-- <span>Pro Admin</span> -->
      <el-menu class="ele-menu" unique-opened :collapse="isColleapsed"
      :collapse-transition="false" router :default-active="activeNav">
      <!-- 一级菜单 -->
        <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
          <template slot="title">
            <i :class="iconList[item.id]"></i>
            <span>{{ item.authName }}</span>
          </template>
          <!-- 二级菜单 -->
          <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children"
          :key="subItem.id" @click="saveNavStatus('/' + subItem.path)">
            <i class="el-icon-menu"></i>
            <span>{{ subItem.authName }}</span>
          </el-menu-item>
        </el-submenu>
      </el-menu>
    </el-aside>
    <el-container>
      <el-header>
        <div class="home-header">
          <el-button class="toggle-btn" @click="toggleCollapse">|||</el-button>
          <el-button class="logout-btn" size="mini" type="info" @click="logout">退出</el-button>
        </div>
      </el-header>
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
      menuList: [],
      iconList: {
        '125': 'iconfont icon-users',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      isColleapsed: false,
      activeNav: ''
    }
  },
  created() {
    this.getMenuList()
    this.activeNav = window.sessionStorage.getItem('activeNav')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$msg.error(res.meta.msg)
      this.menuList = res.data
    },
    toggleCollapse() {
      this.isColleapsed = !this.isColleapsed
    },
    saveNavStatus(active) {
      window.sessionStorage.setItem('activeNav', active)
      this.activeNav = active
    }
  }
}
</script>
<style lang="less" scoped>
.home_container {
  height: 100%;
}
.el-header {
  background-color: #fff;
  padding: 0;
  align-content: center
}
.el-aside {
  background-color: #fff;
  color: #fff;
}
.ele-menu {
  border-right: none
}
.home-header {
  width: 100%;
}
.logout-btn {
  margin-right: 0;
}
.toggle-btn {
  border: none;
  margin-right: 0;
}

.iconfont {
  margin-right: 10px;
}
.el-main {
  background-color: #eaedf1;
}
</style>
