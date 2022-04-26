<template>
  <nav>
    <ul class="tab-wrapper">
      <tab-nav-item 
        v-for="itemNav in taskListOverview" 
        :key="itemNav.name"
        :listName="itemNav.name"
        :listLength="itemNav.length"
        :isActive="currentView === itemNav.name"
        @emit-tab-value="updateCurrentView" 
      />
    </ul>
  </nav>
</template>

<script setup>
import { defineProps, defineEmit } from 'vue'
import TabNavItem from './TabNavItem.vue'

const props = defineProps({
  currentView: {
    type: String,
    default: 'All'
  },
  taskListOverview: {
    type: Array,
    required: true
  }
})

const emit = defineEmit(['update-current-view'])

const updateCurrentView = payload => {
  emit('update-current-view', payload)
}
</script>

<style scoped>
.tab-wrapper {
  display: flex;
  column-gap: 30px;
  list-style: none;
  margin: 45px 0;
}
</style>
