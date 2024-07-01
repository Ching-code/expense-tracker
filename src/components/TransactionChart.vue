<script setup>
import { ref, watch, computed, onMounted, onBeforeUnmount } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const income = computed(() => {
  return props.transactions
    .filter((transaction) => transaction.amount > 0)
    .map((transaction) => ({
      name: transaction.text,
      value: transaction.amount
    }))
})

const expense = computed(() => {
  return props.transactions
    .filter((transaction) => transaction.amount < 0)
    .map((transaction) => ({
      name: transaction.text,
      value: Math.abs(transaction.amount)
    }))
})

const incomeChart = ref(null)
const expenseChart = ref(null)
const chartContainer = ref(null)

let myIncomeChart
let myExpenseChart

const initCharts = () => {
  myIncomeChart = echarts.init(incomeChart.value, 'dark')
  myExpenseChart = echarts.init(expenseChart.value, 'dark')

  const incomeOption = {
    title: {
      text: 'income',
      left: 'center'
    },
    legend: {
      orient: 'vertical',
      left: 'left'
    },
    tooltip: {
      trigger: 'item',
      formatter: '{b} : {c} ({d}%)'
    },
    series: [
      {
        type: 'pie',
        radius: '50%',
        center: ['50%', '50%'],
        label: {
          show: false,
          formatter: '{b}',
          position: 'inside'
        },
        data: income.value,
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      }
    ]
  }
  const expenseOption = {
    title: {
      text: 'expense',
      left: 'center'
    },
    tooltip: {
      trigger: 'item',
      formatter: '{b} : {c} ({d}%)'
    },
    legend: {
      orient: 'vertical',
      left: 'left'
    },
    series: [
      {
        type: 'pie',
        radius: '50%',
        center: ['50%', '50%'],
        label: {
          show: false,
          formatter: '{b}',
          position: 'inside'
        },
        data: expense.value,
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: 'rgba(0, 0, 0, 0.5)'
          }
        }
      }
    ]
  }

  myIncomeChart.setOption(incomeOption)
  myExpenseChart.setOption(expenseOption)
}

const resizeCharts = () => {
  if (myIncomeChart) {
    myIncomeChart.resize()
  }
  if (myExpenseChart) myExpenseChart.resize()
}

let resizeObserver

onMounted(() => {
  initCharts()
  resizeObserver = new ResizeObserver(() => {
    resizeCharts()
  })
  resizeObserver.observe(chartContainer.value)
})
onBeforeUnmount(() => {
  if (resizeObserver) {
    resizeObserver.disconnect()
  }
})

watch([income, expense], ([newIncome, newExpense]) => {
  myIncomeChart.setOption({
    series: [
      {
        data: newIncome
      }
    ]
  })

  myExpenseChart.setOption({
    series: [
      {
        data: newExpense
      }
    ]
  })
})
</script>

<template>
  <div ref="chartContainer" class="chart-container">
    <div ref="incomeChart" style="width: 100%; height: 100%"></div>
  </div>
  <div ref="chartContainer" class="chart-container">
    <div ref="expenseChart" style="width: 100%; height: 100%"></div>
  </div>
</template>

<style scoped></style>
