<template>
  <div class="login-wrap">
    <el-form
      class="login-form"
      label-position="top"
      label-width="80px"
      :model="formData">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formData.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input @keyup.enter.native="handleLogin" type="password" v-model="formData.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleLogin" class="btn" type="primary">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      formData: {
        username: '',
        password: ''
      }
    };
  },

  methods: {
    async handleLogin() {
      var response = await axios.post('http://localhost:8888/api/private/v1/login', this.formData);
      var { data: { meta: { status, msg } } } = response;
      if (status === 200) {
        this.$message.success(msg);
        var token = response.data.data.token;
        sessionStorage.setItem('token', token);
        this.$router.push('/');
      } else {
        this.$message.error(msg);
      }
    }
  }
};
</script>

<style scoped>
 .login-wrap {
  background-color: #324152;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-wrap .login-form {
  background-color: #fff;
  width: 400px;
  border-radius: 5px;
  padding: 30px;
}
.login-wrap .login-form .btn {
  width: 100%;
}
</style>
