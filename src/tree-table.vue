<template lang='pug'>
  el-table(:data='tableArr' border='' :height='height')
    el-table-column(v-if='!attr.hideInTable' v-for='(attr, index) in model' :label='attr.label' :key='attr.prop' :formatter='attr.formatter' align='left' :min-width='index==0?120:0')
      template(slot-scope="scope")
        span(v-if='index==0' :style='{paddingLeft: (scope.row._level?(scope.row._level*15+10):10)+"px"}')
        span.icon(v-if="index==0 && scope.row.children!=null && scope.row.children.length!=0" @click="toggle(scope.$index)" )
          i.far.fa-plus-square(v-if="!scope.row._expanded")
          i.far.fa-minus-square(v-if="scope.row._expanded")
        span(v-text='attr.type=="user"?scope.row.userName:attr.type=="department"?scope.row.departmentName:(attr.formatter?attr.formatter(scope.row, attr.prop, scope.row[attr.prop]):scope.row[attr.prop])')
    el-table-column(fixed="right" label="操作" min-width='110px')
      template(slot-scope="scope")
        el-button(@click="showUpdate(scope.row)" size="small" type='primary') 编辑
        el-button(@click="remove(scope.row)" size="small" type='danger') 删除
</template>

<script>
// 树数组展开
function serialize(arr) {
  var result = []
  for (var o of arr) {
    result.push(o)
    if (o._expanded) {
      result = result.concat(serialize(o.children))
    }
  }
  return result
}

export default {
  props: ['data', 'model', 'height'],
  watch: {
    data() {
      this.setExpand(this.data)
    }
  },
  computed: {
    tableArr() {
      return serialize(this.data)
    }
  },
  methods: {
    remove(row) {
      this.$emit('remove', row)
    },
    setExpand(arr, level = 0) {
      for (var o of arr) {
        this.$set(o, '_level', level)
        if (o.children) {
          this.$set(o, '_expanded', false)
          this.setExpand(o.children, level + 1)
        }
      }
    },
    showUpdate(row) {
      this.$emit('show-update', row)
    },
    toggle(i) {
      this.tableArr[i]._expanded = !this.tableArr[i]._expanded
    },
  }
}
</script>

<style lang='stylus' scoped>
span.icon
  position relative
  i.far
    font-size 16px
    line-height 23px
    margin-right 4px
    position absolute
    right 0px
</style>
