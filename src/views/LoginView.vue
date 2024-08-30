<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
const router = useRouter()

const baseURL = 'https://todolist-api.hexschool.io'

const email = ref('')
const password = ref('')
// const loginSucceedMsg = ref({})

async function login() {
  //檢查是否有空值
  if (email.value === '' || password.value === '') {
    alert('請輸入完整資料')
    return
  }
  const url = `${baseURL}/users/sign_in`
  const data = {
    email: email.value,
    password: password.value
  }
  try {
    const res = await axios.post(url, data)
    console.log('登入成功', res)

    //設定cookie
    document.cookie = `todoToken=${res.data.token};`
    //暫存使用者名
    document.cookie = `todoUser=${res.data.nickname};`
    alert('登入成功')
    router.push('/')
  } catch (error) {
    console.log('登入失敗', error)
    alert(error.response.data.message)
  }
}
</script>

<template>
  <div id="loginLayout">
    <!-- 左邊圖片 -->
    <div id="imgGrp">
      <img
        src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/logo.png"
        alt="logo"
      />
      <img src="/login-img1.png" alt="decoration image" />
    </div>

    <!-- 右邊表格 -->
    <div id="loginWrapper">
      <img
        src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/logo.png"
        alt="logo"
      />
      <h1>最實用的線上待辦事項服務</h1>
      <div class="inputGrp">
        <div class="inputWrapper">
          <label for="email">Email</label>
          <input
            name="email"
            id="email"
            type="email"
            placeholder="請輸入email"
            v-model="email"
            required
          />
          <p>{{ email.value }}</p>
        </div>
        <div class="inputWrapper">
          <label for="password">密碼</label>
          <input
            name="password"
            id="password"
            type="password"
            minlength="6"
            placeholder="請輸入密碼"
            v-model="password"
            required
          />
          {{ password.value }}
        </div>
      </div>
      <button
        style="
          width: 128px;
          height: 47px;
          border-radius: 10px;
          padding: 12px 48px;
          background-color: #333333;
          color: white;
          border: none;
          font-size: 16px;
          font-weight: 700;
          margin: 1rem 0;
        "
        @click="login"
      >
        登入
      </button>
      <button
        @click="router.push('/signup')"
        style="text-decoration: none; color: #333333; font-size: 16px; font-weight: 700"
      >
        註冊帳號
      </button>
    </div>
  </div>
</template>

<style>
#loginLayout {
  width: 100%;
  background-color: #ffd370;
}
#imgGrp {
  display: none;
}

#imgGrp img {
  display: none;
}

#loginWrapper {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#loginWrapper img {
  width: 20rem;
  margin: 2rem 0;
}

#loginWrapper h1 {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 1rem 0 2rem 0;
}

.inputGrp {
  margin: 0 1rem;
}

.inputWrapper {
  display: flex;
  flex-direction: column;
  margin-bottom: 2rem;
}

label {
  font-size: 14px;
  font-weight: 700;
  line-height: 20.27px;
  margin-bottom: 0.5rem;
}

.inputWrapper input {
  width: 304px;
  padding: 12px 16px;
  border-radius: 10px;
  border: none;
  gap: 10px;
}

@media screen and (min-width: 768px) {
  #loginLayout {
    width: 100%;
    background-color: #ffd370;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #imgGrp {
    display: block;
    width: 20rem;
    margin: 0 2rem;
    object-fit: cover;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  #imgGrp img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  #loginWrapper {
    justify-content: center;
    margin: 0 2rem;
  }

  #loginWrapper img {
    display: none;
  }
}
</style>
