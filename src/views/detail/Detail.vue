<template>
   <div>
     <detail-nav-bar></detail-nav-bar>
     <detail-swiper :top-images="topImages"/>
     <detail-base-info :goods="goods"></detail-base-info>
   </div>
</template>

<script>
import DetailNavBar from './child/DetailNavBar'
import DetailSwiper from './child/DetailSwiper'
import DetailBaseInfo from './child/DetailBaseInfo'

import { getDetail, GoodsInfo } from 'network/detail'

export default {
  name: 'Detail',
  data () {
    return {
      id: null,
      topImages: [],
      goods: {}
    }
  },
  created () {
    this.id = this.$route.params.id
    getDetail(this.id).then(res => {
      const data = res.result
      // 获取顶部轮播图
      this.topImages = data.itemInfo.topImages

      // 获取详情基本信息
      this.goods = new GoodsInfo(data.itemInfo, data.columns, data.shopInfo.services)
    }).catch(() => {
      console.log('请求数据出错')
    })
  },
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo
  }
}
</script>

<style scoped>

</style>
