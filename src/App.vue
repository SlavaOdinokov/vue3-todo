<template>
  <main class="main-wrapper">
    <header-todo />

    <task-input @emit-add-task="addTask" />

    <tab-nav 
      :currentView="state.currentView" 
      :taskListOverview="taskListOverview"
      @update-current-view="setView" 
    />
    
    <task-list 
      :tasksInView="tasksInView"
      @emit-toggle-edit="toggleEdit"
      @emit-delete-task="deleteTask"
    />

    <icon-registry />
  </main>
</template>

<script setup>
import { reactive, computed } from 'vue'
import { v4 as uuid } from 'uuid'
import IconRegistry from './components/icon/icon-registry.vue'
import HeaderTodo from './components/HeaderTodo.vue'
import TabNav from './components/TabNav/TabNav.vue'
import TaskList from './components/TaskList.vue'
import TaskInput from './components/TaskInput.vue'

const state = reactive({
  currentView: 'All',
  taskList: []
})

const taskLists = reactive({
  all: computed(() => state.taskList),
  current: computed(() => state.taskList.filter(item => !item.complete)),
  completed: computed(() => state.taskList.filter(item => item.complete))
})

const taskListOverview = reactive([
  { name: 'All', length: computed(() => taskLists.all.length), status: true },
  { name: 'Current', length: computed(() => taskLists.current.length), status: false },
  { name: 'Completed', length: computed(() => taskLists.completed.length), status: false }
])

const tasksInView = computed(() => {
  if (state.currentView === 'Current') {
    return taskLists.current
  } else if (state.currentView === 'Completed') {
    return taskLists.completed
  } else {
    return taskLists.all
  }
})

const setView = value => {
  state.currentView = value
}

const addTask = newTaskInput => {
  state.taskList.push({
    id: uuid(),
    label: newTaskInput,
    complete: false,
    edit: false
  })
}

const deleteTask = id => {
  const index = state.taskList.findIndex(task => task.id === id)
  state.taskList.splice(index, 1)
}

const toggleEdit = id => {
  const index = state.taskList.findIndex(task => task.id === id)
  state.taskList[index].edit = !state.taskList[index].edit
}
</script>

<style>
.main-wrapper {
  max-width: 600px;
  margin: 0 auto;
  padding: 0 10px;
}
</style>
