<template>
    <div class="login-container">
        <el-form autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left"
                 label-width="0px"
                 class="card-box login-form">
            <h3 class="title">系统登录</h3>
            <el-form-item prop="loginName">
                <span class="svg-container"><wscn-icon-svg icon-class="xinrenzhinan"/></span>
                <el-input type="text" v-model="loginForm.loginName" autoComplete="on"
                          placeholder="用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <span class="svg-container"><wscn-icon-svg icon-class="mima"/></span>
                <el-input type="password" @keyup.enter.native="handleLogin" v-model="loginForm.password"
                          autoComplete="on" placeholder="密码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button  v-waves type="primary" style="width:100%;" :loading="loading" @click.native.prevent="handleLogin">
                    登录
                </el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    import { isWscnEmail } from 'utils/validate';
  
    export default {
      name: 'login',
      data() {
        const validatePass = (rule, value, callback) => {
          if (value.length < 6) {
            callback(new Error('密码不能小于6位'));
          } else {
            callback();
          }
        };
        return {
          loginForm: {
            loginName: '',
            password: ''
          },
          loginRules: {
            loginName: [
                { required: true, trigger: 'blur',  message: '请输入用户名' }
            ],
            password: [
                { required: true, trigger: 'blur', validator: validatePass }
            ]
          },
          loading: false
        }
      },
      methods: {
        handleLogin() {
          this.$refs.loginForm.validate(valid => {
            if (valid) {
              this.loading = true;
              this.$store.dispatch('LoginByUser', this.loginForm).then((res) => {
                const data = res.data;
                this.loading = false;
                if(data.code == 1){
                    this.$router.push({ path: '/' });
                }
              }).catch(err => {
                console.log(err);
                this.loading = false;
              });
            } else {
              console.log('error submit!!');
              return false;
            }
          });
        }
      }
    }
</script>

<style rel="stylesheet/scss" lang="scss">
    @import "src/styles/mixin.scss";
    .login-container {
        @include relative;
        height: 100vh;
        background: #2d3a4b url(../../assets/56a46b7590f5672af5b8.jpg) fixed center center;
        input:-webkit-autofill {
            -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
            -webkit-text-fill-color: #fff !important;
        }
        input {
            background: transparent;
            border: 0px;
            -webkit-appearance: none;
            border-radius: 0px;
            padding: 12px 5px 12px 15px;
            color: #fff;
            height: 47px;
        }
        .el-input {
            display: inline-block;
            height: 47px;
            width: 85%;
        }
        .svg-container {
            padding: 6px 5px 6px 15px;
            color: #fff;
        }

        .title {
            font-size: 26px;
            font-weight: 400;
            color: #fff;
            margin: 0px auto 40px auto;
            text-align: center;
            font-weight: bold;
        }

        .login-form {
            position: absolute;
            left: 0;
            right: 0;
            width: 400px;
            padding: 35px 35px 15px 35px;
            margin: 120px auto;
        }

        .el-form-item {
            border:  #fff 1px solid !important;
            background: rgba(0, 0, 0, 0.1);
            border-radius: 5px;
            color: #454545;
        }

        .forget-pwd {
            color: #fff;
        }
        input:-webkit-autofill , textarea:-webkit-autofill, select:-webkit-autofill {
            -webkit-text-fill-color: #ededed !important;  
            -webkit-box-shadow: 0 0 0px 1000px transparent  inset !important;  
            background-color:transparent;  
            background-image: none;  
            transition: background-color 50000s ease-in-out 0s; //背景色透明  生效时长  过渡效果  启用时延迟的时间  
        }  
        input{
             background-color:transparent;
        }
    }

</style>
