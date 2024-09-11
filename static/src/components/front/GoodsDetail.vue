<template>
  <div id="goods-detail">
    <div class="left">
      <img :src="goods.image|imageFliter" alt="">
      <div class="bottom">
        <span class="like">{{goods.likeTimes}} <i class="el-icon-star-on"></i></span>
        <span class="look">{{goods.lookTimes}} <i class="el-icon-s-opportunity"></i></span>
      </div>
    </div>
    <div class="right">
      <div class="top">
        <span class="logo">第一佳小店</span>
        <span class="name">{{goods.name}}</span>
        <span class="container">库存：{{goods.contains}}</span>
      </div>
      <div class="price">
        <div class="title">
          <i class="el-icon-alarm-clock"></i>
          <span class="time">小店秒杀</span>
        </div>
        <div class="origin">原价：<del>{{goods.originPrice}}RMB</del></div>
        <div class="sell">售价：{{goods.sellPrice}}RMB</div>
      </div>
      <div class="address">
        <div class="produce">生产地：{{goods.createAddress_id}}</div>
        <div class="send">发货地：{{goods.sendAddress_id}}</div>
      </div>
      <p class="intro">{{goods.intro}}</p>
      <div class="bottom">
        <el-input-number v-model="num" controls-position="right" @change="handleChange" :min="1" :max="goods.contains"></el-input-number>
        <span class="court" @click="addCourt">加入购物车</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'GoodsDetail',
  created() {
    var _id = this.$route.params._id
    this.detail(_id)
  },
  data() {
    return {
      num: 0,
      loading: null,
      goods: {}
    }
  },
  methods: {
    addCourt() {
      this.openLoading("加入中...")
      this.$axios.post("/api/add/goods/2/court", { goodsId: this.goods._id })
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.$message({
              message: "加入成功",
              type: "success"
            })
          } else {
            this.$message({
              message: res.data.data.info,
              type: "warning"
            })
            this.$router.push("/login")
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
    detail(_id) {
      this.openLoading()
      this.$axios.get("/api/goods/detail/" + _id)
        .then(res => {
          console.log(res.data)
          this.closeLoading()
          if (res.data.code == 200) {
            this.goods = res.data.data
          }
        })
        .catch(err => {
          this.closeLoading()
        })
    },
    handleChange(value) {
      console.log(value);
    }
  }
}

</script>
<style scoped>
#goods-detail .left img {
  width: 100%;
  height: 256px;
}

#goods-detail {
  width: 70%;
  display: flex;
  justify-content: space-between;
  margin: 20px auto;
}

#goods-detail .left {
  width: 30%;
}

#goods-detail .right {
  width: 50%;
}

#goods-detail .left .bottom {
  width: 100%;
  margin: 10px;
  display: flex;
  justify-content: space-between;
  font-size: 19px;
}

#goods-detail .right .top .logo {
  background-color: darkred;
  font-size: 19px;
  color: white;
  border-radius: 5px;
  padding: 4px;
}

#goods-detail .right .price {
  margin-top: 10px;
  margin-bottom: 10px;
  background-color: rgba(255, 255, 0, 0.1);
  height: 70px;
  border-radius: 5px;
  font-size: 15px;
  line-height: 25px;
  padding: 5px;
  color: gray;
}

#goods-detail .right .address {
  margin-top: 20px;
  height: 50px;
  color: gray;
  padding: 5px;
  font-size: 15px;
  line-height: 25px;
}

#goods-detail .right .intro {
  font-size: 15px;
  margin-top: 10px;
  padding: 5px;
}

#goods-detail .right .bottom .court {
  color: white;
  font-size: 30px;
  background-color: #df3033;
  border-radius: 10px;
  padding: 5px;
  margin-left: 20px;
  cursor: pointer;
}

</style>
