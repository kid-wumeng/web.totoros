<template lang="jade">
  .left
    h1 {{ id ? '修改贴子' : '发布新贴子' }}
    c-input(v-model="title", placeholder="标题")
    input-content(v-model="content")
    .row.-right
      c-button(@click="submit") 确认发布
</template>


<script lang="coffee">
  module.exports =
    components:
      'c-input':       require('components/@/input')
      'c-button':      require('components/@/button')
      'input-content': require('components/@/input-content')

    data: ->
      forum: @state['post-modal'].forum
      post:  @state['post-modal'].post
      id:    @state['post-modal'].post?.id    ? 0
      title: @state['post-modal'].post?.title ? ''

    computed:
      content: -> @state['input-content'].content
      relativeSubjects: -> @state['post-modal'].relativeSubjects ? []

    methods:
      submit: ->
        try
          @check()

          data =
            title:        @title
            content:      @content
            relativeSIDs: @relativeSubjects.map (subject) -> subject.id

          if(@id)
            post = await api.call('post.update', @id, data)
            @notify('done', '修改成功')
            @commit('UPDATE_POST', post)
          else
            post = await api.call('post.create', @forum.id, data)
            @notify('done', '发布成功')
            @commit('CREATE_POST', post)

          @commit('post-modal/HIDE')
        catch error
          @notify('fail', error)

      check: ->
        @model.post.checkTitle(@title)
        @model.post.checkContent(@content)
</script>


<style lang="less" scoped>
  .left{
    width: 600px;
    >*{
      margin-bottom: 12px;
      &:last-child{
        margin-bottom: 0;
      }
    }
    >h1{
      font-size: 17px;
    }
  }
</style>