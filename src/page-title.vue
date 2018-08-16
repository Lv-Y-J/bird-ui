<template lang='pug'>
  //-
    @description 页标题组件
    @author 李小龙
    @date 2018/5/31
  .page-title
    slot
    span(v-if='title' v-text='title')
</template>

<script>
export default {
  data() {
    return {
      title: '',
    }
  },
  mounted() {
    if (this.$slots.default) {
      document.title = this.$slots.default[0].text
    } else {
      document.title = this.title = this.$route.name
    }
  }
}
</script>

<style lang='stylus' scoped>
  .page-title
    border-bottom 1px solid #eee
    font-size 16px
    margin 0 -10px
    padding 0 10px 10px
</style>
