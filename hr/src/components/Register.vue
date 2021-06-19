<template>
    <div class="register_container">
            <span class="title_span">人事管理システム</span>
                <el-form ref="registerFormRef" :model="registerForm"  label-width="" class="register_form" :rules="registerFormRules">
                    <el-form-item label="" prop="username">
                        <div class="search1">
                        <el-input v-model="registerForm.username" prefix-icon="el-icon-user" label-width=120px></el-input>
                        </div>
                    </el-form-item>
                    <el-form-item label="" prop="password">
                      <div class="search2">
                        <el-input v-model="registerForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
                      </div>
                    </el-form-item>
                  <el-form-item label="" prop="repassword">
                    <div class="search2">
                      <el-input v-model="registerForm.repassword" prefix-icon="el-icon-lock" type="password"></el-input>
                    </div>
                  </el-form-item>
                  <el-form-item label="" prop="email">
                    <div class="search2">
                      <el-input v-model="registerForm.email" prefix-icon="el-icon-lock"></el-input>
                    </div>
                  </el-form-item>
                  <el-form-item class="btns">
                    <div class="button">
                      <el-button type="success" @click="sendVerifyCode" :disabled="isDisabled">发送验证码</el-button>
                    </div>
                  </el-form-item>
                  <el-form-item label="" prop="verifyCode">
                    <div class="search2">
                      <el-input v-model="registerForm.verifyCode" prefix-icon="el-icon-lock"></el-input>
                    </div>
                  </el-form-item>
                    <el-form-item class="btns">
                      <div class="button">
                        <el-button type="primary" @click="register">注册</el-button>
                        <router-link to='/login'>
                          <el-button type="primary">返回</el-button>
                        </router-link>
                        <el-button type="info" @click="reset">重置</el-button>
                      </div>
                    </el-form-item>
                </el-form>
    </div>
</template>
<script>
export default {
  data () {
    return {
      isDisabled:false,
      registerForm: {
        username: '',
        password: '',
        repassword: '',
        email: '',
        verifyCode: ''
      },
      registerFormRules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 20, message: '用户名必须在3-20个字符之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 3, max: 20, message: '密码必须在3-20个字符之间', trigger: 'blur' }
        ],
        repassword: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 3, max: 20, message: '密码必须在3-20个字符之间', trigger: 'blur' }
        ],
        email: [
          { required: true, message: '邮箱不能为空', trigger: 'blur' }
        ],
        verifyCode: [
          { required: true, message: '验证码不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    register () {
    // 先进行表单预验证
      this.$refs.registerFormRef.validate(async valid => {
        if (valid === true) {
        // 表单预验证通过，向后台发送请求
          this.$http.post('register?username=' + this.registerForm.username + '&pwd=' + this.registerForm.password + '&rpwd=' + this.registerForm.repassword + '&email=' + this.registerForm.email + '&code=' + this.registerForm.verifyCode)
            .then(res => {
              this.$message.success(res.data.msg)
              if (res.data.state === 200) {
                this.$router.push('/login')
              }
            })
            .catch(error => {
              const error2 = error + ''
              this.$message.info('出错了' + ':' + error2)
            })
        }
      })
    },
    reset () {
      this.$refs.registerFormRef.resetFields()
      this.$message.success('已重置注册框')
    },
    sendVerifyCode () {
      this.isDisabled = true
      setTimeout(() => {
        this.isDisabled = false
      }, 5000)
      this.$http.post('email?emailAddress=' + this.registerForm.email)
        .then(res => {
          this.$message.success(res.data.notes)
        })
        .catch(error => {
          const error2 = error + ''
          this.$message.info('出错了' + ':' + error2)
        })
    }
  }
}
</script>
<style lang="less" scoped>
.register_container{
    display:flex;
    justify-content:center;
    align-items:center;
    //background:url(../assets/bgimg.gif)no-repeat fixed top;
    height: 100%;
    background-size: 100% 100%;
}
.title_span{
    position:relative;
    display:block;
    font-size: 35px;
    width: 350px;
    font-family: "Arial","Microsoft YaHei","黑体","宋体",sans-serif;
    left: 28%;
    top: -10%;
    transform: translate(-50%,-150%);
}
.search1{
     display: block;
     justify-content: center;
     align-items: center;
    .el-input {
      width: 20%;
    }
}
.search2{
     display: block;
     justify-content: center;
     align-items: center;
    .el-input {
      width: 20%;
    }
}
.button{
   left: 28%;
   top: -10%;
.btns{
    display:flex;
    justify-content: flex-end;
}
}
.register_form{
    bottom: 15%;
    width: 100%;
    padding: 10px 20px;
    box-sizing: border-box;
}
</style>
