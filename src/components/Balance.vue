<script setup>
const props = defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <h4>你的平衡</h4>
  <h1 id="balance">$ {{ total }}</h1>
</template>
