<template>
  <div>
    <el-carousel trigger="click" class="el-carousel" height="195px">
      <el-carousel-item v-for="(item, index) in banners" :key="index">
        <a :href="item.link">
          <img :src="item.image" alt="">
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
  props: {
    banners: {
      type: Array
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
</style>
