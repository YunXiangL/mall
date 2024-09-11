<template>
  <div id="vip">
    <el-col :span="8" v-for="(vip,index) of vips" :key="index">
      <el-card shadow="hover">
        <img style="width: 300px;height: 100px;" :src="'../../../static/images/vip'+(index+1)+'.jpg'" alt="加载失败">
        <div class="detail">{{vip.name}}-{{vip.level}}RMB
          <el-button type="primary" @click="buyVip(vip._id)">立即购买</el-button>
        </div>
      </el-card>
    </el-col>
  </div>
</template>
<script>
export default {
  name: 'VIP',
  created() {
    this.getAllVipItems()
  },
  data() {
    return {
      loading: null,
      vips: []
    }
  },
  methods: {
    buyVip(_id) {
      console.log(_id)
      this.openLoading("交易中...")
      this.$axios.post("/api/buy/vip", {
          payValue: 1000,
          cutoffValue: 100,
          vipId: _id,

        })
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.$message({
              message: "恭喜您购买成功",
              type: "success"
            })
          } else {
            this.$message({
              message: res.data.data.info,
              type: "warning"
            })
          }
        })
        .catch(err => {
          this.closeLoading()
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
    getAllVipItems() {
      this.openLoading("VIP数据加载中...")
      this.$axios.get("/api/vip")
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.vips = res.data.data.data
          } else {
            this.$message({
              message: res.data.data.info,
              type: "warning"
            })
          }
        }).catch(err => {
          this.closeLoading()
        })
    },
  }
}

</script>
<style scoped>
#vip {
  margin: 20px;
}

#vip .detail {
  width: 100%;
  margin: 20px;
  display: flex;
  justify-content: space-between;
}

</style>
