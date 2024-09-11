<template>
  <div id="personal">
    
    <el-descriptions title="用户信息">
      <el-descriptions-item label="用户名">{{person.name}}</el-descriptions-item>
      <el-descriptions-item label="账号">{{person.account}}</el-descriptions-item>
      <el-descriptions-item label="账户余额">{{person.balance}}</el-descriptions-item>
      <el-descriptions-item label="VIP">
        <el-tag size="small">{{person.vip}}</el-tag>
      </el-descriptions-item>
      <el-descriptions-item label="收货地址">江苏省苏州市吴中区吴中大道 1188 号</el-descriptions-item>
    </el-descriptions>
  </div>
</template>
<script>
export default {
  name: 'Personal',
  created() {
    this.getPersonal()
  },
  data() {
    return {
      loading: null,
      person: {}
    }
  },
  methods: {
    getPersonal() {
      this.openLoading()
      this.$axios.get("/api/self")
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.person = res.data.data
          }
        })
        .catch(err => {
          this.closeLoading()
        })
    },
    openLoading(text = "数据加载...") {
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
<style scoped>
	#personal{
		margin: 50px;
	}
</style>
