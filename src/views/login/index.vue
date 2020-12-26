<template>
  <div class="vue-container">
    <div class="login-container" :style="login_bg">
      <el-form ref="loginForm" :model="loginForm" class="login-form" auto-complete="on" label-position="left">

        <div class="login-title-container">
          <h3 class="login-title-container-text" >Login Form</h3>
        </div>

        <el-form-item prop="username">
          <el-input
              ref="username"
              v-model="loginForm.username"
              placeholder="Username"
              name="username"
              type="text"
              tabindex="1"
              auto-complete="on"
              prefix-icon="el-icon-user"
              size="medium"
          />
        </el-form-item>

        <el-form-item prop="password">
          <el-input
              :key="passwordType"
              ref="password"
              v-model="loginForm.password"
              :type="passwordType"
              placeholder="Password"
              name="password"
              tabindex="2"
              auto-complete="on"
              @keyup.enter.native="handleLogin"
              prefix-icon="el-icon-lock"
              size="medium"
          />
          <span class="show-pwd" @click="showPwd">
          </span>
        </el-form-item>

        <el-button :loading="loading" type="primary" style="width:100%;margin-bottom:30px;" @click.native.prevent="handleLogin" size="small">Login</el-button>

      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '111111'
      },
      login_bg: {
        backgroundImage: "url(" + require("../../assets/login-bg.jpg") + ")",
        backgroundSize: "100% 100%",
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({path: this.redirect || '/'})
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>
<style lang="scss" scoped>
.vue-container {
  min-height: 100%;
  width: 100%;
  .login-container{

    .login-form, .login-title-containe {
      position: relative;
      width: 520px;
      max-width: 100%;
      padding: 160px 35px 0;
      margin: 0 auto;
      overflow: hidden;
    }
  }

}

</style>
