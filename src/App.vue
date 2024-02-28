<template>
  <HeaderApp />
  <div class="container">
    <BalanceApp :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      @deleteTrans="deleteTransaction"
      :transactions="transactions"
    />
    <AddTransaction @transactionSubmited="handleSubmit" />
  </div>
</template>

<script setup>
import HeaderApp from "./components/Header.vue";
import BalanceApp from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";
import { useToast } from "vue-toastification";
const toast = useToast();
const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 100 },
]);
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((tra) => tra.id != id);
  toast.success("Transaction delted succefully");
};
const handleSubmit = (transactionData) => {
  transactions.value.push({
    id: Math.floor(Math.random() * 1000),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  toast.success("Transaction added succesfully");
};
</script>
