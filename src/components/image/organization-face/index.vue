<template lang="jade">
  a.organization-face(:href="getOrganizationPath(organization)" @click.prevent="click")
    photo-frame(v-if="frame")
      cdn-image(:path="path", :ratio="ratio", :square="square", :radius="radius", :circle="circle" cover)
    cdn-image(v-else :path="path", :ratio="ratio", :square="square", :radius="radius", :circle="circle" cover)
</template>


<script lang="coffee">
  module.exports =
    components:
      'photo-frame': require('components/@/photo-frame')
      'cdn-image':   require('components/image/cdn-image')

    props:
      'organization':
        type: Object
        required: true
      'square':
        type: Boolean
        default: false
      'circle':
        type: Boolean
        default: false
      'frame':
        type: Boolean
        default: false
      'prevent':
        type: Boolean
        default: false

    computed:
      id:      -> @organization.id
      width:   -> @organization.face?.width
      height:  -> @organization.face?.height
      version: -> @organization.face?.version
      path:    -> if @version then "organizations/#{@id}/face?v=#{@version}" else ''
      ratio:   -> if @height and @width then @height / @width else 1
      radius:  -> if @square then 3 else 0

    methods:
      click: ->
        if @prevent
          @$emit('click')
        else
          @toOrganizationPage(@organization)
</script>


<style lang="less" scoped>
  .organization-face{
    box-sizing: border-box;
    cursor: pointer;
  }
</style>