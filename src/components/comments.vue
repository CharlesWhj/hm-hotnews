<template>
  <div class="comment">
    <div class="addcomment" v-show='!isFocus'>
      <input type="text" placeholder="写跟帖" @focus="handlerFocus" />
      <span class="comment">
        <i class="iconfont iconpinglun-"></i>
        <em>{{post.comment_length}}</em>
      </span>
      <i class="iconfont iconshoucang" :class="{active:post.has_star}" @click="mycollect()"></i>
      <i class="iconfont iconfenxiang"></i>
    </div>
    <div class="inputcomment" v-show='isFocus'>
        <textarea  ref='commtext' rows="5" @blur='isFocus = false'></textarea>
        <div>
            <span>发送</span>
            <span>取消</span>
        </div>
    </div>
  </div>
</template>

<script>
// 引进api请求方法
import { collect } from '@/api/articaldetail'
export default {
  props: ['post'],
  data () {
    return {
      isFocus: false
    }
  },
  methods: {
    //   获取焦点时触发
    handlerFocus () {
      this.isFocus = !this.isFocus
      setTimeout(() => {
        this.$refs.commtext.focus()
      }, 1)
    },
    async mycollect () {
      // let res = await collect(this.post.id)
      // // console.log(res)
      // if (res.data.message === '收藏成功') {
      //   this.post.has_star = true
      // } else {
      //   this.post.has_star = false
      // }
      // this.$toast(res.data.message)
      // 化简
      let res = await collect(this.post.id)
      this.$toast(res.data.message)
      this.post.has_star = !this.post.has_star
    }
  }
}
</script>

<style lang='less' scoped>
.inputcomment{
    padding: 10px;
    box-sizing: border-box;
    width: 100%;
    display: flex;
    background-color: #fff;
    align-items: flex-end;
    textarea{
        flex: 3;
        background-color: #eee;
        border:none;
        border-radius: 10px;
        padding: 10px;
    }
    div{
        padding: 20px;
    }
    span {
        margin: 5px;
        display: block;
        flex: 1;
        height: 24px;
        line-height: 24px;
        padding: 0 10px;
        background-color: #f00;
        color:#fff;
        text-align: center;
        border-radius: 6px;
        font-size: 13px;
    }
}
.addcomment {
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  margin-top: 20px;
  display: flex;
  text-align: center;
  position: absolute;
  bottom: 0;
  left: 0;
  > input {
    flex: 4;
    height: 30px;
    line-height: 30px;
    border-radius: 15px;
    border: none;
    background-color: #eee;
    padding-left: 20px;
    font-size: 14px;
  }
  i {
    font-size: 20px;
  }
  > span {
    flex: 1;
    position: relative;
    > em {
      position: absolute;
      right: 0;
      top: -5px;
      font-size: 10px;
      background-color: #f00;
      color: #fff;
      border-radius: 5px;
      padding: 3px 5px;
    }
  }
  > i {
    flex: 1;
  }
}
.comment{
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%
}
.active{
  color: red;
  font-weight: 900
}
</style>
