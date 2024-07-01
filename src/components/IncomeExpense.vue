<script setup>
const props = defineProps({
  income: {
    type: Number,
    required: true
  },
  expense: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>收入</h4>
      <p id="money-plus" class="money plus">+$ {{ income }}</p>
    </div>
    <div>
      <h4>支出</h4>
      <p id="money-minus" class="money minus">-$ {{ expense }}</p>
    </div>
  </div>
</template>

<style scoped>
h4 {
  color: white;
}
.inc-exp-container {
  background-color: #272727;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.inc-exp-container > div {
  flex: 1;
  text-align: center;
}

.inc-exp-container > div:first-of-type {
  border-right: 1px solid #fff;
}

.money {
  font-size: 20px;
  letter-spacing: 1px;
  margin: 5px 0;
}

@media screen and (max-width: 768px) {
  .money {
    font-size: 18px;
  }
}

.money.plus {
  color: #2ecc71;
}

.money.minus {
  color: #c0392b;
}
</style>
