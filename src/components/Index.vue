<template>
  <el-container>
    <el-header class='color-danger' style="text-align: right; font-size: 12px;">
      <h1>えっち</h1>
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>View</el-dropdown-item>
          <el-dropdown-item>Add</el-dropdown-item>
          <el-dropdown-item>Delete</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <span>Tom</span>
    </el-header>


    <div class="block">
      <el-carousel id="carousel" trigger="click" interval="1000" loop=true>
        <el-carousel-item class="carousel-item" v-for="item in items" :key="item.id">
          <el-image
            :src="item.image">
          </el-image>
        </el-carousel-item>
      </el-carousel>
    </div>

    <el-row>
      <el-col :xs="24" :md="4">
        <el-button type="danger" v-on:click="shot">出るとこみてて...</el-button>
      </el-col>
      <el-col :xs="24" :md="20">
        <el-progress :text-inside="true" :stroke-width="26" :percentage="100 - shots"></el-progress>
      </el-col>
    </el-row>
    <el-input v-model="query"></el-input>
    <el-button type="info" v-on:click="getNatori">検索</el-button>
  </el-container>
</template>

<script>
const axios = require('axios')

export default {
  name: 'Index',
  data: function() {
    return {
      shots: 0,
      items: [
        {image : "http://jcau.jp/wp-content/uploads/2016/05/testimage.png", id:1},
        {image: "https://www.wcdsb.ca/wp-content/uploads/sites/69/2017/04/testimage-940x400.jpg", id:2},
        {image: "https://www.wcdsb.ca/wp-content/uploads/sites/69/2017/04/testimage-940x400.jpg", id:3},
      ],
      query: "名取さな",
      image: "",
    }
  },

  components: {

  },

  methods: {
    shot: function() {
      this.shots++
    },
    getNatori: function() {
      axios
      .get('https://www.googleapis.com/customsearch/v1?key=AIzaSyDTm-8Cbr_YHU91f4j1bnOkr_tcyQKTwJk&cx=003355543797813723016:weshegao53w&searchType=image&q=' + this.query)
      .then(res => {
        let items = []
        this.image = res.data.items[0].link
        res.data.items.forEach(item => {
          items.push({image: item.link})
        })

        this.items = items
      })
    }
  },
  mounted: function() {
    this.getNatori()
  }
}
</script>

<style scoped lang="sass">
#carousel
  .el-image
    width: 100%
    height: auto
</style>
