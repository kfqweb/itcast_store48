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
        <!-- 组件，vue有自己的事件机制，用的是vue事件机制实现的keyup事件 -->
        <!-- 想使用DOM中的keyup事件 
        native是告诉组件，我要使用原生DOM事件 -->
        <el-input @keyup.enter.native="handleLogin" type="password" v-model="formData.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleLogin" class="btn" type="primary">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
// 导入axios模块
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
    // 处理登录
    async handleLogin() {
      var response = await axios.post('http://localhost:8888/api/private/v1/login', this.formData);

      // var status = response.data.meta.status;
      // var msg = response.data.meta.msg;

      var { data: { meta: { status, msg } } } = response;

      if (status === 200) {
        // 登录成功
        // 提示
        this.$message.success(msg);
        // var { data: data: {token} } = response;
        // 记录token
        var token = response.data.data.token;
        sessionStorage.setItem('token', token);
        // 跳转到后台首页
        this.$router.push('/');
      } else {
        // 登录失败
        this.$message.error(msg);
      }
    }
  }
};
</script>

<style scoped>
/* scoped 是html5中提供的属性

  会给当前页面所有的标签，添加一个data-v-xxx的一个属性作为标示
  当前页面的样式只为当前页面的元素服务
*/
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
