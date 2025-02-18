<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import {ref, computed} from 'vue'

  const transactions = ref([
    {id: 1, text: 'Paycheck', amount: 700.00},
    {id: 1, text: 'Water Bill', amount: -72.83},
    {id: 1, text: 'Electric Bill', amount: -153.89},
    {id: 1, text: 'Returned Item', amount: 20.00},
  ])

  const sum = computed(()=>{
    return transactions.value.reduce((acc, x)=>{
      return acc+x.amount
    },0)
  })

</script>


<template>
  <Header></Header>
  <div class="container">
    <Balance :total="sum"></Balance>
  </div>

</template>