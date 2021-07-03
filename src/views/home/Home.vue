<template>
<div id="home">
  <nav-bar id="home-nav-bar"><div slot="center">购物街</div></nav-bar>
  <scroll class="content" ref="scroll1"
      :probeType="3" :pullUpLoad="true"
      @scrollposition="isShowB2T"
      @pullingUp="loadMore">
    <home-swiper :banners="banner"></home-swiper>
    <home-recommend :recommend="recommond"></home-recommend>
    <feature-view/>
    <tab-control class="table-control" :titles="['流行','新款','精选']" @tabClick="tabClick"></tab-control>
    <good-list :goods="showGoods"></good-list>
  </scroll>
<!--  当你在这个标签上加@click事件的时候报错,原因是因为没有加上native,官网对于native的解释为:
.native - 监听组件根元素的原生事件。-->
  <back-top @click.native="back2Top" v-show="isShow"/>
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodList from 'components/content/goods/GoodList'
import BackTop from 'components/content/BackTop'
import Scroll from '../../components/common/scroll/Scroll'

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
    GoodList,
    BackTop,
    Scroll
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
      currentType: 'pop',
      isShow: false
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

        this.$refs.scroll1.finishPullUp()
      })
    },
    back2Top () {
      this.$refs.scroll1.scrollTo(0, 0)
      // this.refs.scroll.scroll.scrollTo(0, 0, 300)
    },
    isShowB2T (position) {
      // console.log(position)
      if (position.y < -1000) {
        this.isShow = true
      } else {
        this.isShow = false
      }
    },
    loadMore () {
      this.getHomeGoods(this.currentType)

      this.$refs.scroll1.refresh()
    }
  }
}
</script>

<style scoped>
#home {
  /*padding-top: 44px;*/
  height: 100vh;
  position: relative;
  /*top: 44px*/
}

#home-nav-bar {
  background-color: var(--color-tint);
  color: #fff;

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

.content {

  /*overflow: hidden;*/
  /*position: absolute;*/
  /*top: 44px;*/
  /*bottom: 49px;*/
  /*left: 0;*/
  /*right:0;*/
  height: calc(100% - 93px);
  overflow: hidden;
  margin-top: 44px;
}
</style>
