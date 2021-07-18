<template>
   <div id="detail">
     <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>
     <scroll class="content"
             ref="scroll"
             :probe-type="3"
             @scrollposition="contentScroll">
       <detail-swiper :top-images="topImages"/>
       <detail-base-info :goods="goods"></detail-base-info>
       <detail-shop-info :shop="shop"></detail-shop-info>
       <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
       <detail-param-info :param-info="goodsParam" ref="params"></detail-param-info>
<!--       <detail-common-info :detail-info="detailInfo"></detail-common-info>-->
       <detail-comment-info :comment-info="commentInfo" ref="comment"/>
       <good-list :goods="recommends" ref="recommend"/>
     </scroll>
     <detail-bottom-bar @addCart="addToCart"/>
   </div>
</template>

<script>
import DetailNavBar from './child/DetailNavBar'
import DetailSwiper from './child/DetailSwiper'
import DetailBaseInfo from './child/DetailBaseInfo'
import DetailShopInfo from './child/DetailShopInfo'
import DetailGoodsInfo from './child/DetailGoodsInfo'
import DetailParamInfo from './child/DetailParamInfo'
import DetailCommentInfo from './child/DetailCommentInfo'
import DetailBottomBar from './child/DetailBottomBar'

import Scroll from 'components/common/scroll/Scroll'
import GoodList from 'components/content/goods/GoodList'

import { getDetail, GoodsInfo, Shop, GoodsParam, getRecommend } from 'network/detail'
import { itemListenerMixin } from '../../common/minin'
import { debounce } from '../../common/utils'

export default {
  name: 'Detail',
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    GoodList,
    DetailBottomBar
  },
  data () {
    return {
      id: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      goodsParam: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      getThemeTopYs: null,
      currentIndex: 0
    }
  },
  mixins: [itemListenerMixin],
  created () {
    this.id = this.$route.params.id
    getDetail(this.id).then(res => {
      const data = res.result
      // 获取顶部轮播图
      this.topImages = data.itemInfo.topImages

      // 获取详情基本信息
      this.goods = new GoodsInfo(data.itemInfo, data.columns, data.shopInfo.services)

      // 获取商铺信息
      this.shop = new Shop(data.shopInfo)

      // 保存商品详情数据
      this.detailInfo = data.detailInfo

      // 商品参数数据
      this.goodsParam = new GoodsParam(data.itemParams.info, data.itemParams.rule)

      // 获取评论信息
      if (data.rate.cRate !== 0) {
        this.commentInfo = data.rate.list[0]
      }

      // 请求推荐数据
      getRecommend().then(res => {
        // console.log(res)
        this.recommends = res.data.list
      })

      // 4.给getThemeTopY赋值
      this.getThemeTopYs = debounce(() => {
        this.themeTopYs = []
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
        this.themeTopYs.push(Number.MAX_VALUE)
        // console.log(this.themeTopYs)
      }, 100)
    }).catch(() => {
      console.log('请求数据出错')
    })
  },
  methods: {
    imageLoad () {
      this.$refs.scroll.refresh()
      this.getThemeTopYs()
    },
    contentScroll (position) {
      // 1.获取y值
      const positionY = -position.y
      // 2.positionY和主题中值进行对比
      const length = this.themeTopYs.length
      for (let i = 0; i < length - 1; i++) {
        // if (this.currentIndex !== i && ((i < length -1 && positionY >= this.themeTopYs[i] && positionY <
        //   this.themeTopYs[i+1]) || (i === length-1 && positionY > this.themeTopYs[i]))){
        //   this.currentIndex = i;
        //   this.$refs.nav.currentIndex = this.currentIndex;
        // }
        // 优化
        if (this.currentIndex !== i && (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i + 1])) {
          this.currentIndex = i
          this.$refs.nav.currentIndex = this.currentIndex
        }
      }
      // 判断backTop是否显示
      this.isShowBackTop = -position.y > 1000
      // 觉得tabControl是否吸顶(position:fixed)
      this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
    destroyed () {
      this.$bus.$off('itemImgLoad', this.itemImgListener)
    },
    titleClick (index) {
      this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
    },
    addToCart () {
      // 1.获取购物车需要展示的信息添加进去
      const product = {}
      product.image = this.topImages[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.realPrice
      product.iid = this.iid
      // 2.将商品添加到购物车里面
      // this.$store.commit('addCart',product);
      this.$store.dispatch('addCart', product).then(res => {
        // this.show = true;
        // this.message = res;
        // setTimeout(() => {
        //   this.show = false;
        //   this.message = '';
        // },1500)
        // console.log(res)
        // console.log(this.$toast)
        this.$toast.show(res, 2000)
      })
    }
  }
}
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav{
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px);
    position: relative;
  }
</style>
