<template lang="jade">
  row.desc-card
    .left
      slot(name="face")
    .right
      .name {{ name }}
      text-area.desc(:text="desc")
</template>


<script lang="coffee">
  module.exports =
    components:
      'text-area': require('components/@/text-area')

    props:
      'name':
        type: String
        default: ''
      'desc':
        type: String
        default: ''
</script>


<style lang="less" scoped>
  .desc-card{
    .left > *{
      width: 64px;
    }
    .right{
      margin-left: 12px;
    }
    .name{
      font-weight: 600;
      font-size: 15px;
      color: #445669;
    }
    .desc{
      margin-top: 4px;
      font-size: 13px;
      line-height: 19px;
      color: #707C88;
    }
  }
</style>