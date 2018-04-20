<template lang="pug">
	el-upload.image-input(action='' :show-file-list='false', :on-change='handleChange', :auto-upload='false')
		img.avatar(v-if='imgUrl', :src='imgUrl')
		i.el-icon-plus.avatar-uploader-icon(v-else)
		.des(v-text='des' v-if='!imgUrl')
</template>
<script>
export default {
  model: {
    prop: 'model',
    event: 'change'
  },
  props: ['model', 'des', 'lastImg'],
  data() {
    return {
      imgUrl: this.lastImg
    }
  },
  watch: {
    model(val, oldVal) {
      if ((val != null) & (val != '')) {
        this.imgUrl = URL.createObjectURL(val)
      }
    },
    lastImg(val) {
      this.imgUrl = val
    }
  },
  methods: {
    handleChange: function(file) {
      if (file.raw.type.indexOf('image') == -1) {
        this.$message.error('上传只能是图片格式!')
      } else if (file.size / 1024 / 1024 > 2) {
        this.$message.error('上传图片大小不能超过 2MB!')
      } else {
        this.imgUrl = file.url
        this.$emit('change', file.raw)
      }
    }
  }
}
</script>

<style lang='stylus'>
.image-input
  .des
    position absolute
    bottom 0
    right 5px
    color #aaa

  .avatar
    width 178px
    height 178px
    display block

  .avatar-uploader-icon
    font-size 28px
    width 178px
    height 178px
    line-height 178px
    display block

.el-upload
  border 1px dashed #dcdfe6
</style>
