<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import TransactionList from './components/TransactionList.vue';

  import {ref, computed} from 'vue'

  const transactions = ref([])

  const sum = computed(()=>{
    return transactions.value.reduce((acc, x)=>{
      return acc+x.amount
    },0)
  })

  const moneyIn = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount>0)
    .reduce((acc, x)=>{
      return acc+x.amount
    },0)
  })

  const moneyOut = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount<0)
    .reduce((acc, x)=>{
      return acc+x.amount
    },0)
  })

  const handleTransaction = (transactionData) => {
    transactions.value.push({
      text: transactionData.text,
      amount: transactionData.amount,
    })
  }

  const handleDelete = (id) => {
    transactions.value = transactions.value.filter((x) => x.id !== id)
  }

</script>


<template>
  <Header></Header>
  <div class="container">
    <Balance :total="sum"></Balance>
    <IncomeExpenses :income="moneyIn" :expense="moneyOut"></IncomeExpenses>
    <AddTransaction @transactionSubmitted="handleTransaction"></AddTransaction>
    <TransactionList :transactions="transactions" @transactionDeleted="handleDelete"></TransactionList>
  </div>

</template>