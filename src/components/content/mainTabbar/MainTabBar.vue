<template>
    <tab-bar>
        <tab-bar-item path="/home" active-color="#ff5777">
            <img src="~@/assets/img/tabbar/home.svg" slot="img">
            <img src="~@/assets/img/tabbar/home_active.svg" slot="img-sel">
            <div slot="name">
                首页
            </div>
        </tab-bar-item>
        <tab-bar-item path="/category" active-color="#ff5777">
            <img src="~@/assets/img/tabbar/category.svg" slot="img">
            <img src="~@/assets/img/tabbar/category_active.svg" slot="img-sel">
            <div slot="name">
                分类
            </div>
        </tab-bar-item>
        <tab-bar-item path="/shopcart" active-color="#ff5777">
            <img src="~@/assets/img/tabbar/shopcart.svg" slot="img">
            <img src="~@/assets/img/tabbar/shopcart_active.svg" slot="img-sel">
            <div slot="name">
                购物车
            </div>
        </tab-bar-item>
        <tab-bar-item path="/profile" active-color="#ff5777">
            <img src="~@/assets/img/tabbar/profile.svg" slot="img">
            <img src="~@/assets/img/tabbar/profile_active.svg" slot="img-sel">
            <div slot="name">
                我的
            </div>
        </tab-bar-item>
    </tab-bar>
</template>

<script>
import TabBar from '@/components/common/tabbar/TabBar.vue'
import TabBarItem from 'components/common/tabbar/TabBarItem'
export default {
  name: 'MainTabBar',
  components: {
    TabBarItem,
    TabBar
  }
}
</script>

<style scoped>

</style>
