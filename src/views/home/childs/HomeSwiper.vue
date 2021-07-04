<template>
  <div>
    <el-carousel trigger="click" class="el-carousel" height="166px" arrow="always">
      <el-carousel-item v-for="(item, index) in banners" :key="index">
        <a :href="item.link">
          <img :src="item.image" alt="" @load="swiperImgLoad">
        </a>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
// import { SwiperItem, Swiper } from 'components/common/swiper'

export default {
  name: 'HomeSwiper',
  components: {
    // SwiperItem,
    // Swiper
  },
  data () {
    return {
      isLoad: false
    }
  },
  props: {
    banners: {
      type: Array,
      default () {
        return []
      }
    }
  },
  methods: {
    swiperImgLoad () {
      if (!this.isLoad) {
        this.$emit('swiperImgLoad')
        this.isLoad = true
      }
    }
  }
}
</script>

<style scoped>
/*.el-carousel {*/
/*  !*width: 100%;*!*/
/*  display: flex;*/
/*}*/
.el-carousel img {
  width: 100%;
}

/*.el-carousel__item:nth-child(2n) {*/
/*  background-color: var(--color-tint);*/
/*}*/

/*.el-carousel__item:nth-child(2n+1) {*/
/*  background-color: #fff;*/
/*}*/
</style>
