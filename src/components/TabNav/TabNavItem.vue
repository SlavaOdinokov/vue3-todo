<template>
  <li :class="tabItemClassName">
    <button class="tab-item__button" @click="$emit('emit-tab-value', listName)">
      {{ listName }} ({{ listLength }})
    </button>
  </li>
</template>

<script setup>
import { defineProps, computed } from "vue";

const props = defineProps({
  listName: {
    type: String,
    required: true,
  },
  listLength: {
    type: Number,
    required: true,
  },
  isActive: {
    type: Boolean,
    default: false,
  },
});

const tabItemClassName = computed(() => {
  return props.isActive ? "tab-item is-active" : "tab-item";
});
</script>

<style scoped>
.tab-item {
  padding-bottom: 6px;
}
.tab-item:hover .tab-item__button {
  color: #0631f8;
}
.tab-item.is-active {
  border-bottom: 3px solid #0631f8;
}
.tab-item.is-active .tab-item__button {
  color: #2d2d2d;
}
.tab-item__button {
  background: transparent;
  color: #6b6b6b;
  font-family: "Source Sans Pro", sans-serif;
  letter-spacing: 1px;
  font-weight: 700;
  outline: none;
  border: 0;
  cursor: pointer;
  transition: 0.2s;
}
</style>
