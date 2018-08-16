<template lang='pug'>
  //-
    @file 根据模型生成表单弹框（含验证、滚动等的处理）
    @author 李小龙
    @date 2018/6/15
  el-dialog(:title='title' :visible.sync='myVisible' :close-on-click-modal='false' :width='width')
    model-form(v-bind='$props' ref='form')
    .btns(slot='footer')
      el-button(v-if='!hideSave' type='primary' @click='save') 保存
      el-button(@click='myVisible = false') {{hideSave?'关闭':'取消'}}
</template>

<script>
import ModelForm from './model-form'

export default {
  components: {
    ModelForm,
  },
  computed: {
    myVisible: {
      get() {
        return this.visible
      },
      set(val) {
        this.$emit('update:visible', val)
      }
    }
  },
  watch: {
    myVisible(val) {
      if (val == false) {
        var dialogBody = this.$el.querySelector('.el-dialog__body')
        if (dialogBody) dialogBody.scrollTop = 0
        this.$refs.form.clearValidate()
      }
    }
  },
  props: {
    ...ModelForm.props, //继承ModelForm的属性
    hideSave: Boolean,
    title: String,
    visible: Boolean,
    width: String,
  },
  methods: {
    save() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.$emit('save')
        }
      })
    },
  }
}
</script>

<style lang='stylus' scoped>

</style>
