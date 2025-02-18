<script setup>
import { ref, onMounted } from "vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([]);

// Load transactions from localStorage
onMounted(() => {
  const savedTransactions = localStorage.getItem("transactions");
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

// Add new transaction
const addTransaction = (newTransaction) => {
  transactions.value.push(newTransaction);
  saveToLocalStorage();
};

// Delete transaction
const deleteTransaction = (index) => {
  transactions.value.splice(index, 1);
  saveToLocalStorage();
};

// Save to localStorage
const saveToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <div class="bg-gray-100 py-6 flex flex-col justify-center sm:py-12">
    <div class="relative py-3 sm:max-w-xl sm:mx-auto">
      <div
        class="relative px-4 py-10 bg-white mx-8 md:mx-0 shadow rounded-3xl sm:p-10"
      >
        <div class="mx-auto">
          <div class="divide-y divide-gray-200">
            <div
              class="py-8 text-base leading-6 space-y-4 text-gray-700 sm:text-lg sm:leading-7"
            >
              <h1 class="text-2xl font-bold text-center mb-8">
                Expense Tracker
              </h1>
              <AddTransaction @add-transaction="addTransaction" />
              <TransactionList
                :transactions="transactions"
                @delete-transaction="deleteTransaction"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
