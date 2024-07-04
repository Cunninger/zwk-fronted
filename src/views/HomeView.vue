<template>
  <div class="login-container">
    <el-form :model="loginForm" ref="loginForm" :rules="rules" label-width="80px" class="login-form">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="loginForm.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="loginForm.password" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleLogin">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    handleLogin() {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          axios.post('/api/user/login', this.loginForm)
              .then(response => {
                const data = response.data;
                if (data.code === 200) {
                  this.$message.success('登录成功');
                  // 处理登录成功后的逻辑
                } else {
                  this.$message.error(data.message);
                }
              })
              .catch(error => {
                this.$message.error('登录失败');
                console.error(error);
              });
        } else {
          console.log('表单验证失败');
          return false;
        }
      });
    }
  }
};
</script>

<style scoped>
.login-container {
  width: 300px;
  margin: 100px auto;
}

.login-form {
  border: 1px solid #dcdfe6;
  padding: 20px;
  border-radius: 4px;
}
</style>