# Vue3-vanilla

## 使用Vue3重构VanillaWebProjects

## Use   pnpm    Create Vue3  Project 

```sh
pnpm create vite
````

## Project Setup

```sh
pnpm install
```

### Compile and Hot-Reload for Development

```sh
pnpm dev
```

### Compile and Minify for Production

```sh
pnpm build
```

### 安装Element-Plus

```sh
pnpm install element-plus 
```

### 按需引入

```sh
pnpm install -D unplugin-vue-components unplugin-auto-import 
```

### vite.config.js 添加配置文件
```js
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// plugins 中添加以下配置
AutoImport({
    resolvers: [ElementPlusResolver()],
}),
Components({
    resolvers: [ElementPlusResolver()],
})
```
##  [01-form-validator   Vue3 写的第一个页面    O(∩_∩)O哈哈哈~~~](src%2Fviews%2F01-form-validator.vue)

### 先看效果 
![01-form-validator.png](src%2Fassets%2F01-form-validator.png)

### 代码上手
```vue 
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
    max-height: 700px;
    max-width: 400px;
    position:absolute;
    left:0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
    padding: 30px 40px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
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
```

### 总结
* 使用 pnpm创建 Vue3项目 引入Element-Plus  设置按需引入   vite.config.js 添加配置信息
* 使用 Vue3中的 getCurrentInstance()  reactive() toRefs() 新特性 
* 使用 箭头函数  const funName = （） => {}   多用
* 使用 正则表达式 对表单进行验证

## [02-NewYearCountdown  新年倒计时](src%2Fviews%2F02-NewYearCountdown.vue)

### 先看效果
![02-NewYearCountdown.png](src%2Fassets%2F02-NewYearCountdown.png)

### 代码上手
```vue
<script setup>
import { ref } from "vue";

const year = ref('')
const days = ref('')
const hours = ref('')
const minutes = ref('')
const second = ref('')

const defYear = '0000'
const def = '00'

const currentYear = new Date().getFullYear()

const nextYear = new Date(`January 01 ${currentYear + 1} 00:00:00` )

const changeDays = () => {
  const currentTime = new Date();
  const timeLeft = nextYear - currentTime;
  const d = Math.floor(timeLeft / 1000 / 60 / 60 / 24)
  const h = Math.floor(timeLeft / 1000 / 60 / 60) % 24
  const m = Math.floor(timeLeft / 1000 / 60) % 60;
  const s = Math.floor(timeLeft / 1000) % 60;

  year.value = nextYear.getFullYear()
  days.value = d
  hours.value = h < 10 ? '0' + h : h
  minutes.value = m < 10 ? '0' + m : m
  second.value = s < 10 ? '0' + s : s
}

changeDays()
setInterval(changeDays,1000)
</script>

<template>
  <body>
  <div class="year">{{ year !== '' ? year : defYear }}</div>
  <h1>New Year Countdown</h1>
  <div class="count">
    <div class="time">
      <h2>{{ days !== '' ? days : def }}</h2>
      <small>days</small>
    </div>
    <div class="time">
      <h2>{{ hours !== '' ? hours : def }}</h2>
      <small>hours</small>
    </div>
    <div class="time">
      <h2>{{ minutes !== '' ? minutes : def }}</h2>
      <small>minutes</small>
    </div>
    <div class="time">
      <h2>{{ second !== '' ? second : def }}</h2>
      <small>seconds</small>
    </div>
  </div>
  </body>
</template>

<style scoped>

* {
  box-sizing: border-box;
}

body {
  background: url('//assets/picture/fire.png') no-repeat center center;
  background-size: 100% 100%;
  height: 100vh;
  color: #fff;
  font-family: 'Lato', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 0;
  overflow: hidden;
  /**
  加上下面这个 图片撑开整个页面
   */
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
}

body::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

body * {
  z-index: 1;
}

h1 {
  font-size: 60px;
  margin: -80px 0 40px;
}

.year {
  font-size: 200px;
  opacity: 0.2;
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}

.count {
  display: flex;
  transform: scale(2);
}

.time {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 15px;
}

.time h2 {
  margin: 0 0 5px;
}

@media (max-width: 500px) {
  h1 {
    font-size: 45px;
  }

  .time {
    margin: 5px;
  }

  .time h2 {
    font-size: 12px;
    margin: 0;
  }

  .time small {
    font-size: 10px;
  }
}
</style>
```
### 总结
* 使用 ref() 时间格式转换  setInterval() 定时器
* 让背景图占满整个屏幕 再好好看看
