<template>
  <svg aria-hidden="true" class="icon-object" :style="styleObject" fill="currentColor">
    <use :xlink:href="`#${icon}`" />
  </svg>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  icon: {
    type: String,
    required: true
  },
  size: {
    type: [String, Number],
    default: 32
  }
})
  
const styleObject = `height: ${props.size}px; width: ${props.size}px;`

</script>
