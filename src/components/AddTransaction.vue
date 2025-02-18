<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-transaction"]);

const title = ref("");
const amount = ref("");
const type = ref("Income");
const errors = ref({});

const validateForm = () => {
  errors.value = {};

  if (!title.value.trim()) {
    errors.value.title = "Title is required";
  }

  if (!amount.value) {
    errors.value.amount = "Amount is required";
  } else if (Number(amount.value) <= 0) {
    errors.value.amount = "Amount must be greater than 0";
  }

  return Object.keys(errors.value).length === 0;
};

const handleSubmit = () => {
  if (validateForm()) {
    emit("add-transaction", {
      title: title.value,
      amount: Number(amount.value),
      type: type.value,
    });

    // Reset form
    title.value = "";
    amount.value = "";
    type.value = "Income";
  }
};
</script>

<template>
  <form @submit.prevent="handleSubmit" class="flex items-end gap-4">
    <div class="flex-1">
      <label class="block text-sm font-medium text-gray-700 mb-1">Title</label>
      <input
        v-model="title"
        type="text"
        placeholder="Enter title"
        class="w-full p-1 border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
      <span v-if="errors.title" class="absolute text-red-500 text-xs mt-1">{{
        errors.title
      }}</span>
    </div>

    <div class="flex-1">
      <label class="block text-sm font-medium text-gray-700 mb-1">Amount</label>
      <input
        v-model="amount"
        type="number"
        min="0"
        step="0.01"
        placeholder="Enter amount"
        class="w-full p-1 border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
      <span v-if="errors.amount" class="absolute text-red-500 text-xs mt-1">{{
        errors.amount
      }}</span>
    </div>

    <div class="w-32">
      <label class="block text-sm font-medium text-gray-700 mb-1">Type</label>
      <select
        v-model="type"
        class="w-full p-1 border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500"
      >
        <option>Income</option>
        <option>Expense</option>
      </select>
    </div>

    <button
      type="submit"
      class="h-10 px-6 rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 cursor-pointer"
    >
      Add
    </button>
  </form>
</template>
