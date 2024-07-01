<script setup>
import { ref } from 'vue'

const text = ref('')
const amount = ref(0)
const isExpense = ref(false)

const emit = defineEmits(['add-transaction'])

const onSubmit = () => {
  if (!text.value || !amount.value) {
    alert('請輸入文字與總價')
    return
  }

  if (amount.value < 0) {
    alert('請輸入正數')
    return
  }

  if (isExpense.value) {
    amount.value *= -1
  }
  emit('add-transaction', {
    text: text.value,
    amount: amount.value
  })

  text.value = ''
  amount.value = 0
}
</script>

<template>
  <h3>紀錄新一筆帳</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="transactionCategory">
      <span :class="{ active: !isExpense }" @click="isExpense = false">收入</span>
      <span :class="{ active: isExpense }" @click="isExpense = true">支出</span>
    </div>
    <div class="form-control">
      <label for="text">文字</label>
      <input v-model="text" type="text" id="text" placeholder="輸入..." />
    </div>
    <div class="form-control">
      <label for="amount">總共</label>
      <input v-model="amount" type="number" id="amount" placeholder="輸入總價..." />
    </div>
    <button class="btn" type="submit">紀帳</button>
  </form>
</template>

<style scoped>
#form {
  display: flex;
  width: 100%;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.form-control {
  width: 100%;
}
.transactionCategory {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  gap: 10px;
}
.transactionCategory span {
  cursor: pointer;
  background-color: #272727;
  color: #fff;
  font-size: 16px;
  padding: 10px;
  border-radius: 4px;
  text-align: center;
}
.transactionCategory span.active {
  border: 1px solid #fff;
}

label {
  align-self: flex-start;
  color: #fff;
  display: block;
  margin: 10px 0;
}

input {
  border-radius: 4px;
  display: block;
  font-size: 16px;
  padding: 10px;
  width: 100%;
}

.btn {
  cursor: pointer;
  background-color: #272727;
  color: #fff;
  border: 0;
  display: block;
  font-size: 16px;
  margin: 10px 0 30px;
  padding: 10px;
  border-radius: 4px;
  text-align: center;
  width: 50%;
  transition: 0.3s all ease;
}
.btn:hover {
  border: 1px solid #fff;
  transform: scale(1.05);
}
</style>
