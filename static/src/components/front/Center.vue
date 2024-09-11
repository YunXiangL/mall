<template>
  <div id="center">
    <div class="search">
      <el-input placeholder="请输入内容" v-model="input3" class="input-with-select">
        <el-select style="width: 130px;" v-model="searchSelect" slot="prepend" placeholder="请选择">
          <el-option v-for="(tag,index) in tags" :key="index" :label="tag.name" :value="tag._id"></el-option>
        </el-select>
        <el-button slot="append" icon="el-icon-search"></el-button>
      </el-input>
    </div>
    <el-tabs @tab-click="tagClick" :tab-position="'left'" style="height: 550px;">
      <el-tab-pane v-for="(tag,index) of tags" :name="tag._id" :key="index" :label="tag.name">
        <!-- 具体商品信息开始 -->
        <div class="container">
          <div class="item" @click="goodsDetail(goods.id)" v-for="(goods,index) in tableGoodsData" :key="index">
            <img :src="goods.image | imageFliter" alt="">
            <p class="intro">{{goods.intro}}</p>
            <div class="price">
              <del class="origin">{{goods.originPrice}}$</del>
              <span class="sell">{{goods.sellPrice}}$</span>
            </div>
          </div>
        </div>
        <!-- 具体商品信息结束 -->
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
export default {
  name: 'Center',
  created() {
    this.getAllTags()
  },
  data() {
    return {
      loading: null,
      tags: [],
      searchSelect: '',
      tableGoodsData: []
    }
  },
  methods: {
    
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
    tagClick(tag) {
      var tagId = tag.$options.propsData.name
      this.openLoading("数据加载中...")
      this.$axios.post("/api/by/tag/goods", {
          tagId
        })
        .then(res => {
          this.closeLoading()
          if (res.data.code == 200) {
            this.tableGoodsData = res.data.data.data
            console.log(this.tableGoodsData[0])
          }
        })
        .catch(err => {
          this.closeLoading()
        })

    },
    goodsDetail(_id) {
      console.log(_id)
      this.$router.push({
        path: "/goods/detail",
        name: "GoodsDetail",
        params: {
          "_id": _id
        }
      })

    },
    getAllTags() {
      this.$axios.get("/api/goods/type")
        .then(res => {
          console.log(res)
          if (res.data.code == 200) {
            this.tags = res.data.data.data
          } else {

          }
        })
        .catch(err => {
          this.$message.error("数据加载失败")
        })
    },
  }
}

</script>
<style scoped>
#center .tag {
  height: 50px;
  cursor: pointer;
}

#center .search {
  margin: 10px;
}

#center .tag:hover {
  color: darkred;
}

.el-select .el-input {
  width: 130px;
}

.input-with-select .el-input-group__prepend {
  background-color: #fff;
}

#center .container {
  width: 100%;
  margin: 10px;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

#center .container .item {
  width: 15%;
  height: 240px;
  cursor: pointer;
  margin: 5px;
}

#center .container .item img {
  width: 100%;
  height: 123px;
  border-radius: 5px;
  margin: 2px;
}

#center .container .item .intro {
  font-size: 1rem;
  color: #333;
  line-height: 1.4rem;
  word-break: break-all;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  margin-top: 10px;
}

#center .container .item .price {
  margin: 2px;
  margin-top: 20px;
  width: 100%;
  display: flex;
  justify-content: space-between;
}

</style>
