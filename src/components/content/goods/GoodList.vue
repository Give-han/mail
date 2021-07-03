<template>
  <div class="goods">
    <good-list-item v-for="(item, id) in goods" :goodsItem="item" :key="id"/>
  </div>
</template>

<script>

import GoodListItem from './GoodListItem'

export default {
  name: 'GoodList',
  components: {
    GoodListItem
  },
  props: {
    goods: {
      type: Array,
      default () {
        return []
      }
    }
  }
}
</script>

<style scoped>
  .goods {
    display: flex;
    /*允许换行*/
    flex-wrap: wrap;
    /*在弹性盒对象的 <div> 元素中的各项周围留有空白 */
    justify-content: space-around;

    padding: 2px;
  }
</style>
