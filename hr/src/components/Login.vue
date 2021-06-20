<template>
    <div class="login_container">
            <span class="title_span">人事管理システム</span>
                <el-form ref="loginFormRef" :model="loginForm"  label-width="" class="login_form" :rules="loginFormRules">
                    <el-form-item label="" prop="username">
                        <div class="search1">
                        <el-input v-model="loginForm.username" prefix-icon="el-icon-user" label-width=120px></el-input>
                        </div>
                    </el-form-item>
                    <el-form-item label="" prop="password">
                      <div class="search2">
                        <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
                      </div>
                    </el-form-item>
                    <el-form-item class="btns">
                      <div class="button">
                        <router-link to="/register">
                          <el-button type="primary">注册</el-button>
                        </router-link>
                        <el-button type="primary" @click="login">登录</el-button>
                        <el-button type="info" @click="reset">重置</el-button>
                      </div>
                    </el-form-item>
                </el-form>
    </div>
</template>
<script>
import Qs from 'qs'
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123'
      },
      loginFormRules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 20, message: '用户名必须在3-20个字符之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 3, max: 20, message: '密码必须在3-20个字符之间', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
    // 先进行表单预验证
      this.$refs.loginFormRef.validate(async valid => {
        if (valid === true) {
        // 表单预验证通过，向后台发送请求
          this.$http.post('login', Qs.stringify(this.loginForm))
            .then(res => {
              console.log(res.data)
              const token = res.data.token
              this.$message.success('登录成功')
              // 保存token
              sessionStorage.setItem('token', token)
              this.$router.push('/home')
            })
            .catch(error => {
              const error2 = error + ''
              if (error2.indexOf('401') !== -1) { this.$message.error('用户名或密码错误') } else this.$message.error('服务器出错了！')
            })
        }
      })
    },
    reset () {
      this.$refs.loginFormRef.resetFields()
      this.$message.success('已重置登录框')
    }
  }

}
</script>
<style lang="less" scoped>
.login_container{
    display:flex;
    justify-content:center;
    align-items:center;
    background:url(../assets/bgimg.gif)no-repeat fixed top;
    background-color: #555;
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
.login_form{
    bottom: 15%;
    width: 100%;
    padding: 10px 20px;
    box-sizing: border-box;

}
</style>
