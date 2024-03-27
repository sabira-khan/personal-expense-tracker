<script setup lang="ts">
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import TransactionSummary from './components/TransactionSummary.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

interface Transaction {
  id: number;
  text: string;
  amount: number;
}

const transactions = ref<Transaction[]>([]);
const toast = useToast();

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Computed properties
const total = computed(() => transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0));
const income = computed(() => transactions.value.filter(transaction => transaction.amount > 0).reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2));
const expenses = computed(() => transactions.value.filter(transaction => transaction.amount < 0).reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2));

// Submit transaction
const handleTransactionSubmitted = (transactionData: Transaction) => {
  transactions.value.push({ id: generateUniqueId(), text: transactionData.text, amount: transactionData.amount });
  saveTransactionsToLocalStorage();
  toast.success('Transaction added.');
};

// Delete transaction
const handleTransactionDeleted = (id: number) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id);
  saveTransactionsToLocalStorage();
  toast.error('Transaction deleted.');
};

// Generate unique ID
const generateUniqueId = () => Math.floor(Math.random() * 1000000);

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => localStorage.setItem('transactions', JSON.stringify(transactions.value));
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <TransactionSummary :income="+income" :expenses="-expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
