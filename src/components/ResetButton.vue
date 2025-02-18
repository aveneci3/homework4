<script setup>
const emit = defineEmits(['resetCount']);
</script>

<template>
  <button @click="emit('resetCount')">Reset</button>
</template>