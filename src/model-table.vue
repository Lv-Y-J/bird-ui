<template lang='pug'>
  //- 根据模型生成简单的表格（无分页/操作按钮等）
  el-table(:data='data' border='')
    el-table-column(type='index' width='36' v-if='hasIndex')
    el-table-column(v-if='!attr.hideInTable' v-for='attr in model' :label='attr.label' :key='attr.prop' :formatter='attr.formatter' :min-width='attr.minWidth||(attr.type=="image"?100:80)' :sortable='attr.sortable' :prop='attr.prop')
      template(slot-scope="scope")
        .cell-content
          span(v-if='attr.type!="image" && attr.type!="url" && attr.type!="multi-image"' v-text='attr.formatter?attr.formatter(scope.row, attr.prop, scope.row[attr.prop]):scope.row[attr.prop]')
          a(v-if='attr.type=="url"' class="link-style" target='_blank' :href='scope.row[attr.prop]' v-text='attr.formatter?attr.formatter(scope.row, attr.prop, scope.row[attr.prop]):scope.row[attr.prop]' :title="attr.formatter?attr.formatter(scope.row, attr.prop, scope.row[attr.prop]):scope.row[attr.prop]")
          a(v-if='attr.type=="image"' target='_blank' :href='scope.row[attr.prop]')
            img(:src='scope.row[attr.prop]')
          template(v-if='attr.type=="multi-image" && scope.row[attr.prop]!=null')
            a.multi-image(v-for='src in scope.row[attr.prop].split(",")' target='_blank' :href='src')
              img(:src='src')
    el-table-column(v-if='rowOpers' fixed="right" label="操作" :min-width='operWidth||"110px"')
      template(slot-scope="scope")
        el-button(v-for='(oper, $index) in rowOpers' :key='$index' size='small' :type='oper.type||"primary"' @click='oper.handler(scope.row)' v-if='!oper.hide || !oper.hide(scope.row)') {{(typeof oper.text == 'function')?oper.text(scope.row):oper.text}}
</template>

<script>
import Model from 'crud-model-class'

export default {
  props: [
    'data',
    'modelSrc',
    'hasIndex', //是否有序号列
    'operWidth',
    'rowOpers', //行内自定义操作按钮
  ],
  data() {
    return {
      model: new Model(this.modelSrc)
    }
  }
}
</script>

<style lang='stylus' scoped>
</style>
