<template>
  <div id="regist">
    <h1>欢迎您注册</h1>
    <div class="info">
      <div>
        <el-input placeholder="请输入您的手机号" v-model="account"></el-input>
      </div>
      <div>
        <el-input placeholder="请输入密码" v-model="password" show-password></el-input>
      </div>
    </div>
    <div class="btn">
      <el-button style="width:100%;height: 40px;margin-top: 10px;" @click="regist" type="primary">注册</el-button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Regist',
  data() {
    return {
      account: '',
      password: '',
      loading: null,
    }
  },
  methods: {
    regist() {
      if (!this.account) {
        this.$message({
          message: '请输入您的手机号',
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
      this.openLoading("注册中...")
      this.$axios.post("/api/regist", {
          account: this.account,
          password: this.password,
        })
        .then(res => {
          this.closeLoading()
          console.log(res)
          if (res.data.code == 200) {
            this.$router.push("/")
          } else {
            this.$message.error(res.data.data.info)
          }
        })
        .catch(err => {
          this.closeLoading()
          console.log(err)
        })
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
  }
}

</script>
<style scoped="">
#regist {
  width: 80%;
  height: 100%;
  margin: 100px auto;
}

#regist h1 {
  width: 200px;
  margin: 10px auto;
}

#regist .info {
  width: 60%;
  margin: 0 auto;
}
#regist .btn{
  width: 60%;
  margin: 0 auto;
}

#regist .info input {
  margin-top: 20px;
}

</style>
