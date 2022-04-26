<template>
  <div class="task-input-group">
    <input 
      class="task-input" 
      type="text" 
      placeholder="Type a new todo item" 
      v-model="newTaskInput"
      @keyup.enter="emitAddTask"
    />
    <button class="task-button" @click="emitAddTask">
      + Add
    </button>
  </div>
</template>

<script setup>
import { ref, defineEmit } from 'vue'

const newTaskInput = ref('')
const emit = defineEmit(['emit-add-task'])

const emitAddTask = () => {
  if (newTaskInput.value !== '') {
    emit('emit-add-task', newTaskInput.value)
    newTaskInput.value = ''
  }
}
</script>

<style scoped>
.task-input-group {
  display: flex;
}
.task-input {
  padding: 16px;
  font-family: 'Source Sans Pro', sans-serif;
  font-weight: 600;
  color: #959595;
  flex: 1;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
  outline: none;
  border: 1px solid #f6f6f6;
  box-shadow: 2px 2px 8px 0 #c0c0c0;
  letter-spacing: 1px;
}
.task-button {
  cursor: pointer;
  font-family: 'Source Sans Pro', sans-serif;
  padding: 18px 24px;
  background: #0631f8;
  color: #fff;
  font-weight: 900;
  border: 0;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  outline: none;
  box-shadow: 2px 2px 8px 0 #c0c0c0;
  transition: .2s;
}
.task-button:hover {
  background: #0327c5;
}
</style>
