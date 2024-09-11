<template>
  <div id="login">
    <h1>欢迎您登录</h1>
    <div class="info">
      <div>

        <el-input placeholder="请输入账号" v-model="account"></el-input>
      </div>
      <div>
        
        <el-input placeholder="请输入密码" v-model="password" show-password></el-input>
      </div>
    </div>
    <div class="tool">
      <div class="type">
        <el-radio v-model="type" label="admin">管理员</el-radio>
        <el-radio v-model="type" label="user">普通用户</el-radio>
      </div>
      <div class="password">
        <el-link @click="gotoRegist" type="primary">注册账号</el-link>
        <el-link type="danger">忘记密码</el-link>
      </div>
      <div class="btn">
        <el-button style="width:500px;height: 40px;margin-top: 10px;" @click="login" type="primary">登录</el-button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Login',
  created() {
    this.isLogin()
  },
  data() {
    return {
      account: '',
      password: '',
      type: "user",
      loading: null,
    }
  },
  methods: {
    isLogin() {
      this.openLoading()
      this.$axios.post("/api/islogin")
        .then(res => {
          console.log(res)
          if (res.data.code == 200) {
            if (res.data.data.type == "admin") {
              this.$router.push("/admin/index")
            } else {
              this.$router.push("/")
            }
          }
          this.closeLoading()
        })
        .catch(err => {
          console.log(err)
          this.closeLoading()
        })
    },
    gotoRegist() {
      this.$router.push("/regist")
    },
    openLoading(text = "加载...") {
      this.loading = this.$loading({
        lock: true,
        text: text,
        spinner: 'el-icon-loading',
        background: 'rgba(0, 0, 0, 0.7)'
      });
    },
    closeLoading() {
      if (this.loading) {
        this.loading.close()
      }
    },
    login() {
      if (!this.account) {
        this.$message({
          message: '请输入您的账号',
          type: 'warning'
        });
        return
      } else if (!this.password) {
        this.$message({
          message: '请输入您的密码',
          type: 'warning'
        });
        return
      }
      this.openLoading("登录中...")
      this.$axios.post("/api/login", { account: this.account, password: this.password, type: this.type })
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            if (this.type == "admin") {
              this.$router.push("/admin/index")
            } else {
              this.$router.push("/")
            }
          } else {
            this.$message.error(res.data.data.info);
          }
          console.log(res)
        })
        .catch(err => {
          console.log(err)
          this.closeLoading()
        })
    }
  }
}

</script>
<style scoped="">
#login {
  width: 80%;
  height: 100%;
  margin: 100px auto;
}

#login h1 {
  width: 200px;
  margin: 10px auto;
}
#login .info {
  width: 60%;
  margin: 0 auto;
}

#login .info input {
  margin-top: 20px;
}
#login .tool{
  width: 50%;
  margin: 10px auto;
}
#login .tool .type{
  display: flex;
  justify-content: space-between;
}
#login .tool .password{
  display: flex;
  justify-content: space-between;
}
#login .tool .btn{
  display: flex;
  justify-content: space-around;
}

</style>
