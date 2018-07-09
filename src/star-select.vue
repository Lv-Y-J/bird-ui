<template lang='pug'>
.star-select
  .stars
    svg(v-for='i in value' viewBox='0 0 100 95.1' width='24px')
      polygon(fill='#fa0' points='50,0 61.8,36.33 100,36.33 69.9,58.78 80.9,95.1 50,72.65 19.1,95.1 30.1,58.78 0,36.33 38.2,36.33')
  .detail(v-text='detail')
</template>

<script>
export default {
  props: {
    value: Number
  },
  computed: {
    detail() {
      var details = {
        1: '非常差',
        2: '差',
        3: '一般',
        4: '好',
        5: '非常好',
      }
      return details[this.value]
    }
  }
}
</script>

<style lang='stylus' scoped>
.detail
  text-align center
</style>
