<template>
  <div id="regmain-container">
    <div class="other-info"></div>
    <div class="reg-content">
      <span>注册</span>
      <input class="input-username" type="text" placeholder="昵称" v-model="nickname">
      <input class="input-password" type="password" placeholder="密码由6-12数字、字母、符号组成" v-model="password">
      <input class="input-mobile" type="text" placeholder="手机号码" v-model="mobile">
      <div class="mobile-sms">
        <input type="text" class="input-sms" placeholder="手机验证码" v-model="smsCode">
        <button class="get-sms" @click="getSms">点击获取</button>
      </div>
      <div class="agreement">
        <input type="radio" value="true" class="check-agreement" v-model="is_agreement">
        <span class="agreement-content">
          <a href="http://">我已同意《anac使用协议》和《anac隐私协议》</a>
        </span>
      </div>
      <button class="register-button" @click="register">注册</button>
      <router-link to="login" class="jump-login">已有账号，去登录>></router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RegisterMain',
  data(){
    return {
      // 数据初始化
      // userinfo: {
        nickname: '',
        password: '',
        mobile: '',
        smsCode: '',
      // },
      is_agreement: false
    }
  },
  methods: {
     async register(){
      // 双向绑定的数据
      const userinfo = {
        nickname: this.nickname,
        password: this.password,
        mobile: this.mobile,
        smsCode: this.smsCode
      }
      const data = await this.$api.auth.register(userinfo)
      console.log(data)
      const result = data.data
      if(result.code == 800){
        this.$store.commit('set_token',result.token)
        localStorage.setItem('token', result.token)
        // console.log(this.$store.state.token)
        this.$router.push('/')
      }else{
        this.$router.replace('/register')
      }
    },
    getSms(){
      const params = {
        type: 'register',
        mobile: '13438036663'
      }
      const  smsCode = this.$api.auth.getSms(params)
      console.log(smsCode)
    }
  },
  // mounted: {
  //   // this.getSms();
  //   // getSms(){
  //   //   const  smsCode = this.$api.auth.getSms()
  //   //   console.log(smsCode)
  //   // }
  // }
}
</script>

<style lang="scss" scoped>
#regmain-container {
  display: grid;
  grid-area: register-main;
  grid-template-rows: 1.5rem auto;
  place-self: center;
  width: 8rem;
  height: 12.13rem;
  .reg-content {
    display: grid;
    align-self: start;
    justify-self: center;
    width: 4rem;
    // border: 1px solid red;
    input:nth-child(n) {
      border: .01rem solid gray;
      font-size: .2rem;
      border-radius: .02rem;
    }
    span {
      place-self: center;
      font-size: .4rem;
    }

    .input-username {
      // grid-area: l-2;
      margin-top: .3rem;
      height: .4rem;
    }

    .input-password {
      // grid-area: l-3;
      margin-top: .2rem;
      height: .4rem;
    }

    .input-mobile {
      // grid-area: l-4;
      margin-top: .2rem;
      height: .4rem;
    }
    .mobile-sms {
      // border: 1px solid gray;
      margin-top: .2rem;
      height: .4rem;
      border-radius: .02rem;
      .input-sms {
        height: .4rem;
        width: 2.8rem;
      }
      .get-sms {
        font-size: .2rem;
        height: .42rem;
        width: 1.15rem;
        margin-left: .01rem;
        background: #4F943B;
        border-radius: 0.02rem;
        border: 0.01rem solid gray;
      }

    }

    .agreement {
      margin-top: .05rem;
      justify-self: start;
      height: .2rem;
      line-height: .2rem;
      span {
        margin-left: .05rem;
        font-size: .12rem;
      }     
    }

    .register-button {
      margin-top: .1rem;
      height: .42rem;
      background: #4F943B;
      border: 0.01rem solid gray;
      border-radius: 0.02rem;
      font-size: 0.2rem;
      font-weight: bold;
    }

    .jump-login {
      margin-top: .1rem;
      margin-right: .1rem;
      height: .3rem;
      justify-self: end;
      font-size: .12rem;
    }
  }
}
</style>