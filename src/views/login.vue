<template>
  <div>
    <div class="container">
      <div class="close">
        <span class="iconfont iconicon-test"></span>
      </div>
      <div class="logo">
        <span class="iconfont iconnew"></span>
      </div>
      <div class="inputs">
        <myinput
          placeholder="请输入手机号"
          type="text"
          :value="uers.username"
          @input="handleNumber"
          :rules="/^1\d{10}$/"
          err_msg="手机号输入不合法，请输入11位手机号"
        ></myinput>
        <br />
        <myinput placeholder="请输入密码" type="password" v-model="uers.password" :rules="/^\w+$/" err_msg="密码不符合要求"></myinput>
        <mybutton @click="handleLogin" text="登录"></mybutton>
      </div>
      <p class="tips">
        没有账号？
        <a href="#/register">去注册</a>
      </p>
    </div>
  </div>
</template>

<script>
import mybutton from '@/components/loginbtn.vue'
import myinput from '@/components/loginInput.vue'
import { userlogin } from '@/api/users'
export default {
  data () {
    return {
      uers: {
        username: '10086',
        password: '123'
      }
    }
  },
  methods: {
    handleLogin () {
      // console.log(this.uers)
      userlogin(this.uers)
        .then(res => {
          // console.log(res)
          // this.$router.push({ path: '/login' })
          if (res.data.message === '登录成功') {
            localStorage.setItem('hotnews_token', res.data.data.token)
            // 把id储存到本地内存
            localStorage.setItem('userID', res.data.data.user.id)
            this.$router.push({ path: `/personal/${res.data.data.user.id}` })
            // console.log(`${res.data.data.user.id}`)
            // console.log(res)
          } else {
            this.$toast.fail('登录失败，请检查账号密码，重新登录')
          }
        })
        .catch(err => {
          console.log(err)
          this.$toast.fail('请重新登录')
        })
    },
    handleNumber (data) {
      this.uers.username = data
    }
  },
  components: {
    mybutton,
    myinput
  }
}
</script>

<style lang="less" scoped>
.container {
  padding: 20px;
  height: 100vh;
  background-color: rgb(242, 242, 242);
}

.close {
  span {
    font-size: 27 / 360 * 100vw;
  }
}

.logo {
  display: flex;
  justify-content: center;

  span {
    display: block;
    font-size: 126 / 360 * 100vw;
    color: #d81e06;
  }
}

.inputs {
  input {
    margin-bottom: 20px;
  }
}

.tips {
  text-align: right;
  margin-bottom: 20px;

  a {
    color: #3385ff;
  }
}
</style>
