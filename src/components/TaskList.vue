<template>
  <ul class="task-list">
    <li 
      class="task-list-item"
      v-for="task in tasksInView"
      :key="task.id"
    >
      <div class="task-list-checkbox-wrapper">
        <icon v-show="!task.complete" icon="circle" size="15" />
        <icon v-show="task.complete" icon="check-circle" size="15" />
        <input 
          type="checkbox" 
          class="task-list-checkbox" 
          v-model="task.complete" 
          :checked="task.complete" 
        />
      </div>
      <input 
        v-if="task.edit" 
        @keyup.enter="emitToggleEdit(task.id)" 
        class="task-list-edit-input" 
        type="text" 
        v-model="task.label" 
      />
      <p v-else class="task-list-text" :class="task.complete ? 'is-complete' : ''">
        {{ task.label }}
      </p>
      <div class="task-list-cta">
        <p>
          <icon 
            @click="emitToggleEdit(task.id)"
            class="task-list-cta__icon" 
            icon="edit" 
            size="16" 
          />
        </p>
        <p>
          <icon 
            @click="emitDeleteTask(task.id)" 
            class="task-list-cta__icon" 
            icon="delete" 
            size="16" 
          />
        </p>
      </div>
    </li>
  </ul>
</template>

<script setup>
import { defineProps, defineEmit } from 'vue'
import Icon from './icon/icon.vue'

const props = defineProps({
  tasksInView: {
    type: Array,
    required: true
  }
})

const emit = defineEmit(['emit-toggle-edit', 'emit-delete-task'])

const emitToggleEdit = id => {
  emit('emit-toggle-edit', id)
}

const emitDeleteTask = id => {
  emit('emit-delete-task', id)
}
</script>

<style scoped>
.task-list-item {
  display: flex;
  align-items: center;
  padding: 16px;
  border: 1px solid #f6f6f6;
  border-radius: 8px;
  box-shadow: 2px 2px 8px 0 #c0c0c0;
  margin-bottom: 16px;
  transition: .2s;
}
.task-list-item:focus,
.task-list-item:hover {
  border: 1px solid #0631f8;
}
.task-list-item:hover .task-list-cta__icon,
.task-list-item:focus .task-list-cta__icon {
  opacity: 1;
}
.task-list-checkbox-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}
.task-list-checkbox {
  position: absolute;
  left: 0;
  opacity: 0;
}
.task-list-edit-input,
.task-list-text {
  margin-right: 16px;
  font-size: 16px;
  margin-left: 12px;
  font-weight: 700;
  flex: 1;
  border: 0;
  transition: .2s;
}
.task-list-text.is-complete {
  color: #6b6b6b;
  text-decoration: line-through;
}
.task-list-cta {
  display: flex;
  column-gap: 16px;
}
.task-list-cta__icon {
  fill: #2d2d2d;
  transition: .2s;
  cursor: pointer;
  opacity: 0;
}
.task-list-cta__icon:hover {
  fill: #0728bf;
}
</style>
