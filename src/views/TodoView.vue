<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'
import TodoComponent from '../components/TodoComponent.vue'

const baseURL = 'https://todolist-api.hexschool.io'
const router = useRouter()

const nickname = document.cookie.replace(/(?:(?:^|.*;\s*)todoUser\s*=\s*([^;]*).*$)|^.*$/, '$1')

const token = document.cookie.replace(/(?:(?:^|.*;\s*)todoToken\s*=\s*([^;]*).*$)|^.*$/, '$1')

const todoBtn = ref('all')
const newTodo = ref('')

const todos = ref([])
const doneTodos = ref([])
const yetTodos = ref([])

function todosStatusChange() {
  doneTodos.value = todos.value.filter((todo) => todo.status === true)
  yetTodos.value = todos.value.filter((todo) => todo.status === false)
}

//刪除cookie
function deleteCookie(cookiname) {
  var d = new Date()
  d.setDate(d.getDate() - 1)
  var expires = ';expires=' + d
  var value = ''
  document.cookie = cookiname + '=' + value + expires + '; path=/'
}

//驗證是否有登入
onMounted(async () => {
  const url = `${baseURL}/users/checkout`

  try {
    await axios.get(url, {
      headers: {
        Authorization: token
      }
    })
    getTodos()
  } catch (error) {
    alert('請先登入')
    router.push('/login')
  }
})

//登出
async function logout() {
  const url = `${baseURL}/users/sign_out`
  try {
    await axios.post(
      url,
      {},
      {
        headers: {
          Authorization: token
        }
      }
    )

    //刪除cookie
    deleteCookie('todoToken')
    deleteCookie('todoUser')

    alert('登出成功')
    router.push('/login')
  } catch (error) {
    console.log('登出失敗', error)
  }
}

// 切換待辦事項顯示
function filterTodos(type) {
  todoBtn.value = type
}

// 取得待辦事項
async function getTodos() {
  const url = `${baseURL}/todos/`

  try {
    const res = await axios.get(url, {
      headers: {
        Authorization: token
      }
    })
    todos.value = res.data.data
    todosStatusChange()
  } catch (error) {
    console.log('拿todos失敗', error)
  }
}

// 新增待辦事項
async function addTodo() {
  const url = `${baseURL}/todos/`

  if (newTodo.value === '') {
    alert('請輸入待辦事項')
    return
  } else {
    try {
      await axios.post(
        url,
        {
          content: newTodo.value
        },
        {
          headers: {
            Authorization: token
          }
        }
      )
      alert('新增成功')
      getTodos()
      newTodo.value = ''
    } catch (error) {
      console.log('新增失敗', error)
    }
  }
}

// 刪除待辦事項
function deleteTodo(id) {
  const url = `${baseURL}/todos/${id}`

  try {
    axios.delete(url, {
      headers: {
        Authorization: token
      }
    })
    alert('待辦事項已刪除')
    getTodos()
  } catch (error) {
    console.log('刪除失敗', error)
  }
}

//toggle todo status
function updateTodoStatus(id) {
  const url = `${baseURL}/todos/${id}/toggle`

  try {
    axios.patch(url, null, {
      headers: {
        Authorization: token
      }
    })

    alert('待辦事項狀態已更新')
    getTodos()
  } catch (error) {
    console.log('toggle 失敗', error)
  }
}
</script>

<template>
  <header>
    <nav>
      <img
        src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/logo.png"
        alt="logo"
      />
      <div class="navWrapper">
        <p>{{ nickname }}的待辦</p>
        <button @click="logout">登出</button>
      </div>
    </nav>
  </header>
  <div id="mainTodoWrapper">
    <!-- input todo -->
    <div class="textInput">
      <input type="text" placeholder="請輸入待辦事項" v-model="newTodo" />
      <button @click="addTodo">+</button>
    </div>

    <!-- display todo -->
    <div v-show="todos.length > 0" id="todoWrapper">
      <div class="buttonGrp">
        <button :class="{ btnActive: todoBtn === 'all' }" @click="filterTodos('all')">全部</button>
        <button :class="{ btnActive: todoBtn === 'yet' }" @click="filterTodos('yet')">
          待完成
        </button>
        <button :class="{ btnActive: todoBtn === 'done' }" @click="filterTodos('done')">
          已完成
        </button>
      </div>
      <!-- map all todos -->
      <div id="todos" v-show="todoBtn === 'all'" v-for="todo in todos" :key="todo.id">
        <TodoComponent @updateTodoStatus="updateTodoStatus" :todo="todo" @deleteTodo="deleteTodo" />
      </div>
      <!-- map yet todos -->
      <div id="todos" v-show="todoBtn === 'yet'" v-for="todo in yetTodos" :key="todo.id">
        <TodoComponent @updateTodoStatus="updateTodoStatus" :todo="todo" @deleteTodo="deleteTodo" />
      </div>
      <!-- map done todos -->
      <div id="todos" v-show="todoBtn === 'done'" v-for="todo in doneTodos" :key="todo.id">
        <TodoComponent @updateTodoStatus="updateTodoStatus" :todo="todo" @deleteTodo="deleteTodo" />
      </div>
      <p class="todoSumText">{{ yetTodos.length }} 個待完成項目</p>
    </div>

    <!-- UI for no todos -->
    <div
      id="noTodoUI"
      style="margin-top: 2rem; display: flex; flex-direction: column; align-items: center"
      v-show="todos.length === 0"
    >
      <h2>目前暫無待辦事項</h2>
      <img src="/img2.png" alt="no todo image" />
    </div>
  </div>
</template>

<style>
nav {
  background-color: #ffd370;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem 0;
}

nav img {
  width: 15rem;
}

/* .navWrapper {
  display: flex;
  font-size: 1.2rem;
  font-weight: 700;
  margin-top: 1rem;
} */

.navWrapper p {
  display: none;
}

nav button {
  font-size: 1.2rem;
  margin-left: 1rem;
}

nav button:hover {
  background-color: #333333;
  color: white;
}

#mainTodoWrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #ffd370;
  /* background-image: linear-gradient(175deg, #ffd370 60%, #fff 40%); */
}

#todoWrapper {
  width: 80%;
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 1rem 0;
}

#todoWrapper .todoSumText {
  margin-top: 1rem;
  margin-left: 2rem;
  font-size: 14px;
}

.textInput {
  position: relative;
  width: 80%;
  margin: 2rem 0;
  font-size: 14px;
}

.textInput input {
  width: 100%;
  height: 40px;
  position: relative;
  border: none;
  border-radius: 10px;
  font-size: 14px;
  padding: 5px;
}

.textInput button {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: none;
  background-color: #333333;
  color: white;
  font-size: 30px;
  position: absolute;
  cursor: pointer;
  top: 5px;
  right: -5px;
}

.buttonGrp {
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.buttonGrp button {
  width: 100%;
  height: 100%;
  font-size: 14px;
  font-weight: 700;
  color: lightgray;
  border-bottom: 2px solid lightgray;
}

.buttonGrp button.btnActive {
  color: #333333;
  border-bottom: 2px solid #333333;
}

#todos {
  width: 100%;
}

#noTodoUI h2 {
  font-size: 1rem;
  margin: 1rem 0;
}
#noTodoUI img {
  width: 15rem;
}

@media screen and (min-width: 768px) {
  nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem 0;
  }
  nav img {
    width: 15rem;
    margin-left: 2rem;
  }

  .navWrapper {
    display: flex;
    font-size: 1.2rem;
    font-weight: 700;
    padding: 0 2rem;
  }

  .navWrapper p {
    display: inline-block;
  }

  nav button {
    font-size: 1rem;
  }

  #mainTodoWrapper {
    background-image: linear-gradient(175deg, #ffd370 60%, #fff 40%);
  }

  #todoWrapper {
    width: 30rem;
  }

  .textInput {
    margin-top: 5rem;
    width: 30rem;
  }

  .textInput input {
    font-size: 16px;
  }

  .textInput button {
    font-size: 24px;
  }

  .buttonGrp {
    height: 40px;
  }

  .buttonGrp button {
    font-size: 16px;
  }

  #noTodoUI h2 {
    font-size: 1.5rem;
  }

  #noTodoUI img {
    width: 20rem;
  }
}
</style>
