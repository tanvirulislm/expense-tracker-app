<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["delete-transaction"]);
const filter = ref("all");

const filteredTransactions = computed(() => {
  if (filter.value === "income") {
    return props.transactions.filter((t) => t.type === "Income");
  }
  if (filter.value === "expense") {
    return props.transactions.filter((t) => t.type === "Expense");
  }
  return props.transactions;
});

const handleDelete = (index, transaction) => {
  if (
    confirm(
      `Are you sure you want to delete transaction "${transaction.title}"?`
    )
  ) {
    emit("delete-transaction", index);
  }
};
</script>

<template>
  <div class="mt-8">
    <div class="flex gap-6 mb-4">
      <label
        v-for="option in ['all', 'income', 'expense']"
        :key="option"
        class="inline-flex items-center cursor-pointer"
      >
        <input
          type="radio"
          :value="option"
          v-model="filter"
          class="form-radio h-4 w-4 text-blue-600 transition duration-150 ease-in-out"
        />
        <span class="ml-2 text-sm font-medium text-gray-700">
          {{ option.charAt(0).toUpperCase() + option.slice(1) }}
        </span>
      </label>
    </div>

    <div v-if="!transactions.length" class="text-gray-500 text-center py-4">
      No transactions recorded yet.
    </div>

    <div v-else class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Title
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Amount
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Type
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Action
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-for="(transaction, index) in filteredTransactions" :key="index">
            <td class="px-6 py-4 whitespace-nowrap">{{ transaction.title }}</td>
            <td
              class="px-6 py-4 whitespace-nowrap"
              :class="[
                transaction.type === 'Income'
                  ? 'text-green-600'
                  : 'text-red-600',
                Number(transaction.amount) >= 500 ? 'font-bold' : '',
              ]"
            >
              ${{ transaction.amount }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap uppercase">
              {{ transaction.type }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <button
                @click="handleDelete(index, transaction)"
                class="px-4 py-2 text-sm font-medium text-white bg-red-600 rounded-md hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 cursor-pointer"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
