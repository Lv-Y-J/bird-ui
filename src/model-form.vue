<template lang='pug'>
  //-
    @file 根据模型生成表单（带el-form及验证），需要ModelInputs注册为全局组件
    @author 李小龙
    @date 2018/6/7
  el-form(ref='form' :model='form' :inline='inline' :label-width='labelWidth || "100px"' :rules='myModel.rules')
    model-inputs(:form='form' :model='myModel' :is-insert='mode=="insert"' :is-edit='mode=="edit"' :is-info='mode=="info"')
</template>

<script>
import Model from 'crud-model-class'

export default {
  computed: {
    myModel() {
      return this.model || new Model(this.modelSrc)
    },
  },
  props: {
    form: Object,
    inline: Boolean,
    labelWidth: String,
    mode: String, //处于哪种模式，可选值：insert|edit|info
    model: Array,
    modelSrc: Object,
  },
  methods: {
    //向上传递el-form的方法
    clearValidate() {
      this.$refs.form.clearValidate()
    },
    validate(cb) {
      return this.$refs.form.validate(cb)
    },
  }
}
</script>

<style lang='stylus' scoped>

</style>
