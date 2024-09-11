<template>
  <div id="balance">
  	<div class="balance">当前账户余额：{{balance}}</div>
  	<div><el-input v-model.number="money" placeholder="请输入要充值的金额"></el-input></div>
  	<div><el-button type="primary" @click="topUp">立即充值</el-button></div>
  </div>
</template>
<script>
export default {
  name: 'Balance',
  created() {
  	this.getBalance()
  },
  data() {
    return {
      loading: null,
      balance:0,
      money:1000
    }
  },
  methods: {
  	topUp(){
  		this.openLoading("充值中...")
  		this.$axios.post("/api/balance",{balance:this.money})
  		.then(res=>{
  			this.closeLoading()
  			if(res.data.code==200){
  				this.$message({
  					message:"充值成功",
  					type:"success"
  				})
  				this.balance = this.balance + this.money
  				this.money = 0
  			}else{
  				this.$message({
  					message:res.data.data.info,
  					type:"warning"
  				})
  			}
  		})
  		.catch(err=>{
  			this.closeLoading()
  		})
  	},
    getBalance() {
      this.openLoading()
      this.$axios.get("/api/self")
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.balance = res.data.data.balance
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
#balance {
  margin: 50px;
}

</style>
