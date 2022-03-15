<template>
  <div v-if="Object.keys(commentInfo).length!=0" class="comment-info">
    <div class="comment-title">
      <div>用户评价</div>
      <div>更多>></div>
    </div>

    <div class="comment-user">
      <img :src="commentInfo.user.avatar" alt="">
      <span>{{commentInfo.user.uname}}</span>
    </div>

    <div class="comment-content">
      <p>{{commentInfo.content}}</p>
      <div class="content-info">
        <span>{{commentInfo.created | formatDate}}</span>
        <span>{{commentInfo.style}}</span>
      </div>
      <div class="content-img">
        <img v-for="(item,index) in commentInfo.images" :key="index" :src="item" alt="">
        <img v-for="(item,index) in commentInfo.images" :key="index" :src="item" alt="">
        <img v-for="(item,index) in commentInfo.images" :key="index" :src="item" alt="">
        <img v-for="(item,index) in commentInfo.images" :key="index" :src="item" alt="">
      </div>
    </div>
  </div>
</template>

<script>
import {formatDate} from "utils/date";

export default {
  name: "DetailCommentInfo",
  props:{
    commentInfo:{
        type: Object,
        default (){
          return {}
        }
      }
    },
  filters: {
    formatDate(time) {
      const date = new Date(time * 1000)
      console.log(new Date(time*1000))
      return formatDate(date, 'yyyy-MM-dd')
    }
    }
  }
</script>

<style scoped lang="stylus">
  .comment-info
    margin-top 15px
    padding 0 8px
    border-bottom 5px solid #f2f5f8
    .comment-title
      font-size 15px
      color black
      display flex
      justify-content space-between
      padding-bottom 15px
      border-bottom 2px solid #f2f5f8
    .comment-user
      display flex
      align-items center
      margin 15px 0
      img
        height 42px
        border-radius 50%
      span
        color black
        margin-left 15px
    .comment-content
      font-size 14px
      p
        margin-bottom 10px
      .content-info
        margin-bottom 15px
        color #999
      .content-img
        margin-bottom 30px
        img
          width 15%
          height 54px
          margin-right  5px
</style>