<template>
<div>
  <nav-bar id="home-nav-bar"><div slot="center">购物街</div></nav-bar>
  <home-swiper :banners="banner"></home-swiper>
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import HomeSwiper from './childs/HomeSwiper'

import { getHomeMutiData } from '../../network/home.js'

export default {
  name: 'Home',
  components: {
    NavBar,
    HomeSwiper
  },
  data () {
    return {
      banner: [],
      recommond: []
    }
  },
  created () {
    getHomeMutiData().then((res) => {
      this.banner = res.data.banner.list
      this.recommond = res.data.recommend.list
    })
  }
}
</script>

<style scoped>
#home-nav-bar {
  background-color: var(--color-tint);
  color: #f6f6f6;
}
</style>
