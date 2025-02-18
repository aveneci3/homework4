<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import TransactionList from './components/TransactionList.vue';
  import Counter from './components/Counter.vue';
  import Display from './components/Display.vue';
  import ResetButton from './components/ResetButton.vue';

  import {ref, computed, onMounted} from 'vue';

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
      id: Date.now(),
      text: transactionData.text,
      amount: transactionData.amount,
    })
    saveToLocalStorage()
  }

  const handleDelete = (id) => {
    transactions.value = transactions.value.filter((x) => x.id !== id)
    saveToLocalStorage()
  }

  const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }

  onMounted( () => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

    if(savedTransactions){
      transactions.value = savedTransactions
    }
  })
  
  const count = ref(0);

  const updateCount = (newCount) => {
    count.value = newCount;
  };

  const resetCount = () => {
    count.value = 0;
  };



</script>


<template>
  <Header></Header>
  <div class="container">
    <Balance :total="sum"></Balance>
    <IncomeExpenses :income="moneyIn" :expense="moneyOut"></IncomeExpenses>
    <AddTransaction @transactionSubmitted="handleTransaction"></AddTransaction>
    <TransactionList :transactions="transactions" @transactionDeleted="handleDelete"></TransactionList>
    <Display :count="count"/>
    <Counter @updateCount="updateCount"/>
    <ResetButton @resetCount="resetCount"/>
  </div>

</template>