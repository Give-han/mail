<template>
<div id="home">
  <nav-bar id="home-nav-bar"><div slot="center">购物街</div></nav-bar>
  <home-swiper :banners="banner"></home-swiper>
  <home-recommend :recommend="recommond"></home-recommend>
  <feature-view/>
  <tab-control class="table-control" :titles="['流行','新款','精选']" @tabClick="tabClick"></tab-control>
  <good-list :goods="showGoods"></good-list>
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodList from 'components/content/goods/GoodList'

import HomeSwiper from './childs/HomeSwiper'
import HomeRecommend from './childs/HomeRecommend'
import FeatureView from './childs/FeatureView'

import { getHomeMutiData, getHomeGoods } from '../../network/home.js'

export default {
  name: 'Home',
  components: {
    NavBar,
    HomeSwiper,
    HomeRecommend,
    FeatureView,
    TabControl,
    GoodList
  },
  data () {
    return {
      banner: [],
      recommond: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentType: 'pop'
    }
  },
  computed: {
    showGoods () {
      return this.goods[this.currentType].list
    }
  },
  created () {
    getHomeMutiData().then((res) => {
      this.banner = res.data.banner.list
      this.recommond = res.data.recommend.list
    })
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  methods: {
    /**
     * 事件监听相关的方法
     */
    tabClick (index) {
      switch (index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }
    },
    getHomeGoods (type) {
      const page = this.goods.pop.page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        // this.$refs.scroll.finishPullUp()
      })
    }
  }
}
</script>

<style scoped>
#home {
  padding-top: 44px;
  /*height: 100vh;*/
  /*position: relative;*/
}

#home-nav-bar {
  background-color: var(--color-tint);
  color: #f6f6f6;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}

.tab-control {
  position: sticky;
  top: 44px;
  z-index: 9;
}
</style>
