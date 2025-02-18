<script setup>
import { ref, defineEmits } from 'vue';

const count = ref(0);
const emit = defineEmits(['updateCount']);

const increment = () => {
  count.value++;
  emit('updateCount', count.value);
};
</script>

<template>
  <div>
    <button @click="increment">Increase Count</button>
  </div>
</template>