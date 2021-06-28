<template>
    <div class="tab-bar-item" @click="itemClick">
        <div v-if="!isActive">
            <slot name="img"></slot>
        </div>
        <div v-else>
            <slot name="img-sel"></slot>
        </div>
        <div id="active" :style="activeStyle">
            <slot name="name"></slot>
        </div>
    </div>
</template>

<script>
export default {
  name: 'TabBarItem',
  props: {
    path: String,
    activeColor: {
      type: String,
      default: 'pink'
    }
  },
  data () {
    return {
    }
  },
  computed: {
    isActive () {
      return this.$route.path.indexOf(this.path) !== -1
    },
    activeStyle () {
      return this.isActive ? { color: this.activeColor } : {}
    }
  },
  methods: {
    itemClick () {
      this.$router.replace(this.path).catch(() => {})
    }
  }
}
</script>

<style scoped>

    .tab-bar-item {
        flex: 1;
        text-align: center;
        height: 49px;
    }

    .tab-bar-item img {
        height: 24px;
        width: 24px;
        margin-top: 3px;
        margin-bottom: 2px;
        vertical-align: middle;
    }
</style>
