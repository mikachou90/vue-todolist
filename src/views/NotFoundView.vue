<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()
</script>

<template>
  <div id="notFound">
    <h2>Oops...Error occurred!!</h2>
    <p>你來到不存在的頁面了，趕快回到地球去!</p>
    <button @click="router.push('/')">回 todo list</button>
  </div>
</template>

<style>
#notFound {
  background-color: #ffd370;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-size: 2rem;
}

#notFound button {
  font-size: 1.2rem;
  margin-top: 3rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #333333;
  color: white;
}

#notFound h2 {
  margin: 2rem 0;
}
</style>
