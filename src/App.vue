<script setup lang="ts">
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import TransactionSummary from './components/TransactionSummary.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
</script>

<template>
  <Header />
  <div class="container">
    <Balance />
    <TransactionSummary />
    <TransactionList />
    <AddTransaction />
  </div>
</template>
