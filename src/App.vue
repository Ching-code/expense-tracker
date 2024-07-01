<script setup>
import Header from '@/components/Header.vue'
import Balance from '@/components/Balance.vue'
import Income from '@/components/IncomeExpense.vue'
import TransactionList from '@/components/TransactionList.vue'
import AddTransaction from '@/components/AddTransaction.vue'
import TransactionChart from '@/components/TransactionChart.vue'

import { ref, computed, onMounted } from 'vue'

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const addTransaction = (transaction) => {
  transactions.value.push({ id: transactions.value.length + 1, ...transaction })
  saveTransactionsToStorage()
}

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  saveTransactionsToStorage()
}

const saveTransactionsToStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

const total = computed(() => {
  return transactions.value.reduce((sum, cur) => sum + cur.amount, 0)
})

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((sum, cur) => sum + cur.amount, 0)
})

const expense = computed(() => {
  return Math.abs(
    transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((sum, cur) => sum + cur.amount, 0)
  )
})
</script>

<template>
  <Header></Header>
  <div class="container">
    <div class="item">
      <Balance :total="total"></Balance>
      <Income :income="income" :expense="expense"></Income>
      <TransactionList
        :transactions="transactions"
        @delete-transaction="deleteTransaction"
      ></TransactionList>
    </div>
    <div class="item">
      <AddTransaction @add-transaction="addTransaction"></AddTransaction>
    </div>
  </div>
  <div class="container">
    <TransactionChart :transactions="transactions"></TransactionChart>
  </div>
</template>

<style scoped></style>
