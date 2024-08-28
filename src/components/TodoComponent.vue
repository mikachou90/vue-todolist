<script setup>
import { defineEmits } from 'vue'
defineProps(['todo'])

const emit = defineEmits('deleteTodo', 'updateTodoStatus')
</script>

<template>
  <div id="todo.id" class="todoWrapper">
    <button
      v-show="todo.status === true"
      class="donecheck"
      @click="emit('updateTodoStatus', todo.id)"
    >
      <svg x="0px" y="0px" viewBox="0 0 507.506 507.506" width="20" height="20">
        <g>
          <path
            d="M163.865,436.934c-14.406,0.006-28.222-5.72-38.4-15.915L9.369,304.966c-12.492-12.496-12.492-32.752,0-45.248l0,0   c12.496-12.492,32.752-12.492,45.248,0l109.248,109.248L452.889,79.942c12.496-12.492,32.752-12.492,45.248,0l0,0   c12.492,12.496,12.492,32.752,0,45.248L202.265,421.019C192.087,431.214,178.271,436.94,163.865,436.934z"
            fill="#FFD370"
          />
        </g>
      </svg>
    </button>

    <button
      v-show="todo.status === false"
      class="checkbox"
      @click="emit('updateTodoStatus', todo.id)"
    ></button>
    <p :class="{ checktext: todo.status }">{{ todo.content }}</p>
    <button class="deleteBtn" @click="emit('deleteTodo', todo.id)">X</button>
  </div>
</template>

<style>
.todoWrapper {
  display: flex;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid lightgray;
  position: relative;
}

.todoWrapper .icon {
  width: 20px;
  height: 20px;
  margin-right: 1rem;
  color: blueviolet;
}

.todoWrapper .checkbox {
  width: 20px;
  height: 20px;
  border-radius: 5px;
  border: 1px solid #333333;
  margin-right: 1rem;
}

.todoWrapper .deleteBtn {
  position: absolute;
  right: 1rem;
  font-size: 1rem;
  color: #333333;
  cursor: pointer;
}

.todoWrapper .deleteBtn:hover {
  font-size: 1.2rem;
}

.checktext {
  text-decoration: line-through;
  color: lightgray;
}

.donecheck {
  padding: 0;
  margin-right: 1rem;
}
</style>
