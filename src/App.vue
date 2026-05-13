<template>
  <div style="padding: 20px;">
    <h2>登录系统</h2>

    <div>
      <input v-model="username" placeholder="用户名" />
    </div>

    <div style="margin-top: 10px;">
      <input v-model="password" type="password" placeholder="密码" />
    </div>

    <div style="margin-top: 10px;">
      <button @click="login">登录</button>
    </div>

    <p style="margin-top: 10px;">{{ message }}</p>

    <p v-if="token">
      当前Token：{{ token }}
    </p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const message = ref('')
const token = ref(localStorage.getItem('token') || '')

async function login() {
  message.value = '登录中...'

  try {
    const res = await fetch('http://127.0.0.1:5000/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        username: username.value,
        password: password.value
      })
    })

    const data = await res.json()

    if (res.ok && data.token) {
      token.value = data.token
      localStorage.setItem('token', data.token)
      message.value = '登录成功'
    } else {
      message.value = '登录失败'
    }

  } catch (err) {
    message.value = '请求失败（后端未启动或跨域问题）'
  }
}
</script>