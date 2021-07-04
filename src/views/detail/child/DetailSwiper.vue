<template>
  <div class="detail-swiper">
    <el-carousel trigger="click" class="el-carousel" height="200px" arrow="always">
      <el-carousel-item v-for="(item, index) in topImages" :key="index" class="el-carousel">
        <img :src="item"
             alt="">
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
export default {
  name: 'DetailSwiper',
  props: {
    topImages: {
      type: Array,
      default () {
        return []
      }
    }
  }
}
</script>

<style scoped>
  .el-carousel img {
    width: 100%;
  }

  /*.detail-swiper {*/
  /*  height: 200px;*/
  /*  overflow: hidden;*/
  /*}*/
</style>
