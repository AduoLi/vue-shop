<template>
  <div class="login-main">
    <div class="login-box">
      <div class="user-head">
        <img src="../assets/head.jpg" alt srcset />
      </div>

      <el-form ref="loginFromRef" :model="loginFrom" label-width="0px" :rules="loginFromRules" class="login-from">
        <el-form-item prop="username">
          <el-input v-model="loginFrom.username" prefix-icon="iconfont icon-users"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="loginFrom.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
          ></el-input>
        </el-form-item>
        <el-form-item class="btn">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginFrom">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginFrom: {
        username: "admin",
        password: "123456",
      },
      loginFromRules: {
        username: [
          { required: true, message: "请输入名称", trigger: "blur" },
          {
            min: 2,
            max: 10,
            message: "长度在 3 到 10 个字符",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
      resetLoginFrom() {
          this.$refs.loginFromRef.resetFields()
      },
      login() {
          this.$refs.loginFromRef.validate((valid) => {
              if(!valid) return
              this.$http.post('login', this.loginFrom).then(res => {
                console.log(res.data)
                if(res.data.meta.status != 200) {
                  this.$message.error('登录失败') 
                  return
                }
                this.$message({message:'登录成功', type:'success'})
                window.sessionStorage.setItem('token', res.data.data.token)
                this.$router.push('/home')
              })
          })
      }
  },
};
</script>

<style lang="less" scoped>
.login-main {
  width: 100%;
  height: 100%;
  background-color: #2b4b6b;
  background: url('../assets/indexbg.jpg') no-repeat center/cover;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 5px;
  position: relative;
  .user-head {
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.login-from {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btn {
  display: flex;
  justify-content: flex-end;
}
</style>