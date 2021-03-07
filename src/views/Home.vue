<template>
  <el-container class="home_container">
    <el-header>
      <div>
        <img src="../assets/logo.png" alt="">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>

      <!-- 侧栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle_botton" @click="toggleCollapse">|||</div>
        <el-menu 
        background-color="#333744" 
        text-color="#fff" 
        active-text-color="#409EFF"
        :unique-opened="true"
        :collapse="isCollapse"
        :collapse-transition="false"
        router
        :default-active="'/'+ activePath"
        >
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
            <!-- 一级菜单 -->
            <template slot="title">
              <i :class="icon[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item 
            v-for="items in item.children" 
            :key="items.id"
            :index="'/'+items.path"
            @click="saveNavState(items.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{items.authName}}</span>
              </template>
            </el-menu-item>

          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main><router-view/></el-main>
    </el-container>
  </el-container>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        menuList: [],
        icon:{
          '125': 'el-icon-user-solid',
          '103': 'el-icon-box',
          '101': 'el-icon-s-goods',
          '102': 'el-icon-s-order',
          '145': 'el-icon-s-platform',
        },
        isCollapse:false,
        activePath:''
      }
    },
    created() {
      this.getMenuList()
      this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods: {
      // 退出登录
      logout() {
        window.sessionStorage.clear('token')
        this.$router.push('/login')
      },
      // 获取数据
      async getMenuList() {
        const {data: res} = await this.$http.get('menus')
        res.meta.status !== 200 ?
        this.$message.error(res.meta.msg) :
        this.menuList = res.data
        // console.log(res);
      },
      // 控制折叠
      toggleCollapse() {
        this.isCollapse = !this.isCollapse
      },
      // 保存连接的激活状态
      saveNavState(activePath) {
        window.sessionStorage.setItem('activePath', activePath)
        this.activePath = this.activePath
      }
    },
  
  }

</script>

<style lang="scss" scoped>
  .el-header {
    background-color: #373d41;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #fff;
    font-size: 20px;

    >div {
      display: flex;
      align-items: center;

      span {
        margin-left: 15px;
      }

      img {
        height: 50px;
      }
    }
  }

  .el-aside {
    background-color: #333744;
    transition: all 0.5s;

    .el-menu {
      border-right: solid 0px #e6e6e6;
    }
  }

  .el-main {
    background-color: #eaedf1;
  }

  .home_container {
    height: 100%;
  }
  .toggle_botton {
    background-color: #4A5064;
    font-style: 10px;
    line-height: 24px;
    text-align: center;
    color: #fff;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
</style>
