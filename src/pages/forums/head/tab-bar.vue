<template lang="jade">
  tab-bar.forum-tab-bar(:tabs="tabs", :active="active", @change="change")
</template>


<script lang="coffee">
  module.exports =
    components:
      'tab-bar': require('components/@/tab-bar')

    props:
      'forum':
        type: Object
        required: true

    data: ->
      forums: []

    computed:
      tabs: ->
        return @forums.map (forum) =>
          return
            label: forum.name
            value: forum.id
            count: @getPostCount(forum) + @getCommentCount(forum)
            link:  "/forums/#{forum.id}"

      active: -> @forum.id

    created: ->
      @init()

    activated: ->
      @init()

    methods:
      init: ->
        @forums = await api.call('forum.getAll', {open: true})

      change: (tab) ->
        @$router.replace('/forums/' + tab.value)

      getPostCount: (forum) ->
        today = forum.todayByPost ? new Date()
        now   = new Date()
        if(today.getFullYear() is now.getFullYear() and today.getMonth() is now.getMonth() and today.getDate() is now.getDate())
          return forum.todayPostCount ? 0
        else
          return 0

      getCommentCount: (forum) ->
        today = forum.todayByComment ? new Date()
        now   = new Date()
        if(today.getFullYear() is now.getFullYear() and today.getMonth() is now.getMonth() and today.getDate() is now.getDate())
          return forum.todayCommentCount ? 0
        else
          return 0
</script>


<style lang="less">
  .forum-tab-bar{
    margin: 0 auto;
    width: 800px;
    position: relative;
    top: 1px;
    .tab{
      padding: 12px 28px;
      font-weight: 600;
      border-bottom: 2px dotted transparent;
      .label{
        font-size: 14px;
        color: #A2AEBA;
      }
      .count{
        margin-left: 6px;
        font-size: 13px;
        color: #d9534f;
        opacity: 0.5;
        &::before{
          content: "+";
        }
      }
    }
    .tab:not(.-active):hover{
      border-bottom: 2px solid #445669;
    }
    .tab.-active{
      font-weight: 600;
      border-bottom: 2px solid #445669;
      .label{
        color: #445669;
      }
      .count{
        color: #d9534f;
        opacity: 1;
      }
    }
  }
</style>