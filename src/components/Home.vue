<template>
    <el-container class="home-container">
      <el-header>
        <div>
          <span>电商后台管理系统</span>
        </div>
        <el-button type="info" @click="logout">退出</el-button>
      </el-header>
      <el-container>
        <el-aside :width="isCollapse ? '64px' : '200px'">
          <div class="toggle-button" @click="toggleCollapse">|||</div>
          <el-menu background-color="#333744" text-color="#fff" active-text-color="409EFF" unique-opened
          :collapse="isCollapse" :collapse-transition="false" router :default-active="activepath">
      <el-submenu :index="item.id+''" v-for="item in menulist" :key="item.id">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>{{ item.authName }}</span>
        </template>
      <el-menu-item :index="'/'+subItem.path+''" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/'+subItem.path)">
        <template slot="title">
          <i class="el-icon-menu"></i>
          <span>{{ subItem.authName }}</span>
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
  data () {
    return {
      menulist: [],
      isCollapse: false,
      activepath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activepath = window.sessionStorage.getItem('activepath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    saveNavState (activepath) {
      window.sessionStorage.setItem('activepath', activepath)
      this.activepath = activepath
    }
  }
}
</script>

<style lang="less" scoped>
.home-container{
  height: 100%;
}
.el-header{
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 10px;
  align-items: center;
  color: #fff;
  font-size: 20px;
  >div{
    display: flex;
    align-items: center;
  }
}
.el-aside{
  background-color: #333744;
  .el-menu{
    border-right: none;
  }
}
.el-main{
  background-color: #eaedf1;
}
.iconfont{
  margin-right: 10px;
}
.toggle-button{
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2rem;
  cursor: pointer;
}
</style>
