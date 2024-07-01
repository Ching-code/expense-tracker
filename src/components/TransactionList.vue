<script setup>
import { ref, reactive, computed } from 'vue'

const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['delete-transaction'])

const deleteTransaction = (id) => {
  emit('delete-transaction', id)
}

const formattedTransactions = computed(() => {
  return props.transactions.map((transaction) => {
    const formattedAmount =
      transaction.amount < 0
        ? `-$${Math.abs(transaction.amount)}`
        : `+$${Math.abs(transaction.amount)}`
    return {
      ...transaction,
      amount: formattedAmount,
      isPositive: transaction.amount > 0
    }
  })
})
</script>

<template>
  <h3>記錄歷史</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in formattedTransactions"
      :key="transaction.id"
      :class="transaction.isPositive ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }}</span>
      <button class="del-btn" @click="deleteTransaction(transaction.id)">-</button>
    </li>
  </ul>
</template>

<style scoped>
h3 {
  border-bottom: 1px solid #fff;
}
.list {
  list-style-type: none;
  padding: 0;
  margin-bottom: 40px;
}

.list li {
  background-color: #272727;
  box-shadow: var(--box-shadow);
  color: #fff;
  display: flex;
  justify-content: space-between;
  position: relative;
  padding: 10px;
  margin: 10px 0;
}
.list li:hover {
  box-shadow: 0 0 20px 2px rgba(255, 255, 255, 0.8);
}

.list li.plus {
  border-right: 5px solid #2ecc71;
}

.list li.minus {
  border-right: 5px solid #c0392b;
}

.del-btn {
  cursor: pointer;
  background-color: #e74c3c;
  color: #fff;
  width: 30px;
  height: 30px;
  font-size: 20px;
  line-height: 30px;
  border: none;
  border-radius: 50%;
  position: absolute;
  top: calc(50% - 15px);
  right: -40px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.list li:hover .del-btn {
  opacity: 1;
}
</style>
