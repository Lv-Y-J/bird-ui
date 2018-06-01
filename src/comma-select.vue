<template lang='pug'>
  //- 根据逗号分隔的名称和id生成下拉框
  el-select(v-model='myValue' :multiple='multiple' collapse-tags)
    el-option(v-for='(name, $index) in myNames' :key='myIds[$index]' :value='myIds[$index]' :label='name')
</template>

<script>
export default {
  model: {
    prop: 'value',
    event: 'change'
  },
  props: ['value', 'names', 'ids', 'multiple'],
  computed: {
    myValue: {
      get() {
        return this.value
      },
      set(v) {
        this.$emit('change', v)
      },
    },
    myNames() {
      return this.names.replace(/,$/, '').split(',')
    },
    myIds() {
      return this.ids.replace(/,$/, '').split(',')
    },
  },
}
</script>

<style lang='stylus' scoped>

</style>
