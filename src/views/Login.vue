<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <!-- 登录表单 -->
      <el-form 
      class="login_from" 
      :model="loginFrom"
      :rules="rules"
      ref="loginFrom"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input 
          prefix-icon="el-icon-user-solid"
          v-model="loginFrom.username"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input 
          prefix-icon="el-icon-lock"
          v-model="loginFrom.password"
          type="password"
          ></el-input>
        </el-form-item>
        <!-- 登录按钮 -->
        <el-form-item class="btn">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="reset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Login',
    data() {
      return {
        loginFrom: {
          username:'admin',
          password:'123456'
        },
        rules:{
          username:[
            { required:true, message: '请输入用户名', trigger: 'blur'}
          ],
          password: [
            { required:true, message: '请输入密码', trigger: 'blur'}
          ]
        }
      }
    },
    components: {},
    methods: {
      reset() {
        this.$refs.loginFrom.resetFields()
      },
      login() {
        this.$refs.loginFrom.validate(async valid => {
          if(!valid) return;
          const {data: res} = await this.$http.post('login', this.loginFrom)
          res.meta.status !== 200 ?
          this.$message.error('登录失败') : 
          this.$message.success('登陆成功')
          window,sessionStorage.setItem("token", res.data.token)
          this.$router.push('/home')
          console.log(res);
        })
      }
    },
    mounted() {

    },
  }

</script>

<style lang="scss" scoped>
  .login_container {
    height: 100%;
    background-color: #2b4b6b;
    // display: flex;
    // align-items: center;
    // justify-content: center;
  }

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avatar_box {
      width: 100px;
      height: 100px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      background-color: #fff;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);

      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }

  .login_from{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }

  .btn{
    display: flex;
    justify-content: center;
  }
</style>
