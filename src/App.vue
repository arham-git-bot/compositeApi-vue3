<template>
  <HeaderComponent />
  <div class="container">
    <BalanceComponent :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransitionComponent
      :transitions="transitions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransition @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
<script setup>
import HeaderComponent from "./components/HeaderComponent.vue";
import BalanceComponent from "./components/BalanceComponent.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransitionComponent from "./components/TransitionComponent.vue";
import AddTransition from "./components/AddTransition.vue";

import { ref, computed, onMounted } from "vue";

import { useToast } from "vue-toastification";
const toast = useToast();

const transitions = ref([]);

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transitions"));

  if (savedTransaction) {
    transitions.value = savedTransaction;
  }
});

// Total
const total = computed(() => {
  return transitions.value.reduce((acc, transition) => {
    return acc + transition.amount;
  }, 0);
});

// income
const income = computed(() => {
  return transitions.value
    .filter((transition) => transition.amount > 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0)
    .toFixed(2);
});

//expenses
const expenses = computed(() => {
  return transitions.value
    .filter((transition) => transition.amount < 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0)
    .toFixed(2);
});
const generateUniquqId = () => {
  return Math.floor(Math.random() * 100);
};
const handleTransactionSubmitted = (transactionData) => {
  transitions.value.push({
    id: generateUniquqId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedtransactionToLocalStorage();
  toast.success("Added Successfully");
};
const handleTransactionDeleted = (id) => {
  transitions.value = transitions.value.filter(
    (transition) => transition.id !== id
  );
  toast.success("Deleted Successfully");
};

// save to local storage
const savedtransactionToLocalStorage = () => {
  localStorage.setItem('transitions', JSON.stringify(transitions.value));
};
</script>
