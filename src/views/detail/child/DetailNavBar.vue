<template>
  <div>
    <nav-bar>
      <div slot="left" class="left"
           @click="backClick">
        <img src="~@/assets/img/common/back.svg">
      </div>
      <div slot="center" class="center">
        <div v-for="(item, index) in titles"
             :key="index"
             class="title"
             :class="{active: index === currentIndex}"
             @click="itemClick(index)">
          <span>{{item}}</span>
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'

export default {
  name: 'DetailNavBar',
  components: {
    NavBar
  },
  data () {
    return {
      titles: ['商品', '参数', '评论', '推荐'],
      currentIndex: 0
    }
  },
  methods: {
    itemClick (index) {
      this.currentIndex = index
      this.$emit('titleClick', index)
    },

    backClick () {
      this.$router.back()
    }
  }
}
</script>

<style scoped>
  .center {
    display: flex;
  }

  .title {
    flex: 1;
    font-size: 13px;
  }

  .left img {
    margin: 11px auto 11px;
  }

  .active {
    color: var(--color-high-text);
  }
</style>
