<template lang='pug'>
//单选弹出框
.select-popup
  el-button(:type='myBtnStyle[0]' :size='myBtnStyle[1]' @click='showDialog') {{showSelected?(multi?lastSelectedObj.map(s => s.name).join(','):(lastSelectedObj.name||'请选择')):myBtnStyle[2]}}
  el-dialog(:title='myBtnStyle[2]' :visible.sync='dialogShow' append-to-body)
    el-radio(v-if='!multi && showEmpty' v-model='selected' :key='null' :label='emptyObj' border) 请选择
    el-radio(v-if='!multi' v-model='selected' v-for='option in options' :key='option.id' :label='option' border) {{option.name}}
    el-checkbox(v-if='multi' v-model='selected' v-for='option in options' :key='option.id' :label='option' border) {{option.name}}
    .dialog-footer(slot='footer')
      el-button(type='primary', @click='save') 确定
      el-button(type='cancel', @click='dialogShow = false') 取消
</template>

<script>
var emptyObj = {}

export default {
  model: {
    prop: 'value',
    event: 'change'
  },
  props: [
    'btnStyle',  //覆盖默认按钮样式，[类型, 大小, 文字]
    'options',
    'multi',  //多选
    'showSelected', //将已选项名称显示在按钮中
    'showEmpty',  //显示无值的请选择
    'value',
    'prop', //v-model里绑定哪个字段，不传时绑定整个对象
  ],
  watch: {
    value(v) {
      if (!this.multi) {
        var sel = this.options.filter(op => op[this.prop] == v)[0]
        if (sel) {
          this.selected = sel
        } else {
          this.selected = emptyObj
        }
        this.lastSelectedObj = this.selected
      }
    }
  },
  data() {
    return {
      emptyObj,
      myBtnStyle: this.btnStyle || ['primary', 'medium', '选择'],
      dialogShow: false,
      selected: this.multi ? [] : emptyObj,
      lastSelectedObj: this.multi ? [] : emptyObj,
    }
  },
  mounted() {
  },
  methods: {
    save() {
      this.dialogShow = false
      this.lastSelectedObj = this.selected
      this.$emit('change', this.prop == null ? this.selected : (this.multi ? this.selected.map(s => s[this.prop]) : this.selected[this.prop]))
      this.$emit('change-obj', this.selected)
    },
    showDialog() {
      this.$emit('show-dialog', this)
      this.dialogShow = true
    },
  }
}
</script>

<style lang='stylus' scoped>
.el-dialog {
  .el-radio, .el-checkbox {
    margin: 10px;
  }
}

.dialog-footer {
  .el-button {
    width: 80px;
  }
}
</style>
