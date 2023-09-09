<script setup>
import { reactive, toRefs } from "vue";
import { getCurrentInstance } from 'vue';

const { proxy } = getCurrentInstance();

const data = reactive({
  form: {
    username: '',
    email: '',
    password: '',
    pwd: ''
  },
  rules: {
    username: [
      { required: true, message: 'Please enter username', trigger: 'blur' }
    ],
    email: [
      { required: true, message: 'Please enter email', trigger: 'blur' },
      { pattern: /\w[-\w.+]*@([A-Za-z0-9][-A-Za-z0-9]+\.)+[A-Za-z]{2,14}/, message: '请输入正确的邮箱格式', trigger: 'blur' }
    ],
    password: [
      { required: true, message: 'Please enter password', trigger: 'blur' },
      { pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/, message: '长度至少为8位 字母 数字 特殊字符 @$!%*?&', trigger: 'blur' }
    ],
    pwd: [
      { required: true, message: 'Please enter password', trigger: 'blur' },
      { pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/, message: '长度至少为8位 字母 数字 特殊字符 @$!%*?&', trigger: 'blur' }
    ]
  }
})

const { form, rules } = toRefs(data)

proxy.resetForm = (refName) => {
  if (proxy.$refs[refName]) {
    proxy.$refs[refName].resetFields();
  }
}

proxy.reset = () => {
  form.value = {
    username: undefined,
    email: undefined,
    password: undefined,
    pwd: undefined
  },
      proxy.resetForm('registerRef')
}

const submit = () =>{
  proxy.$refs.registerRef.validate(valid => {
    if (form.value.password !== form.value.pwd) {
      console.log('password:' + form.value.password + '  pwd:' + form.value.pwd)
      // 这里不要导入 import {ElMessage} from "element-plus";
      // 否则 element 样式效果会消失
      // 博客地址：https://juejin.cn/post/7247428110163984421
      // 解决办法： 在tsconfig.json中的include字段中添加auto-imports.d.ts就可以了,这样就会自动引入了。
      // "include": ["env.d.ts", "src/**/*", "src/**/*.vue", "auto-imports.d.ts"],
      ElMessage.error('两次输入的密码必须相同')
    }
    if (valid && form.value.password === form.value.pwd) {
      console.log('password:' + form.value.password + '  pwd:' + form.value.pwd)
      ElMessage({ message: '注册成功', type: 'success' })
      proxy.reset()
      // 携带 注册信息 走登录接口
    }
  })
}
</script>

<template>
  <div>
    <el-form  ref="registerRef" :model="form" :rules="rules">
      <h2>Register With Us</h2>
      <label>Username</label>
      <el-form-item  prop="username"></el-form-item>
      <el-input v-model="form.username" placeholder="Please enter username"></el-input>
      <label>Email</label>
      <el-form-item  prop="email"></el-form-item>
      <el-input v-model="form.email" placeholder="Please enter email"></el-input>
      <label>Password</label>
      <el-form-item prop="password"></el-form-item>
      <el-input v-model="form.password" type="password" placeholder="Please enter password"></el-input>
      <label>Confirm Password</label>
      <el-form-item prop="pwd"></el-form-item>
      <el-input v-model="form.pwd" type="password" placeholder="Please enter username again"></el-input>
      <el-button type="primary" @click="submit" >Register</el-button>
    </el-form>
  </div>
</template>

<style scoped>
h2 {
  text-align: center
}
.el-form {
  height: 700px;
  width: 400px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  padding: 30px 40px;
  border: 3px solid #9d4fec;
}
.el-input {
  height: 50px;
  margin: 0 0 30px;
}
.el-button {
  width: 400px;
  height: 50px;
  margin: 20px 0;
}
</style>
