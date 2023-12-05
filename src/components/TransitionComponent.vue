<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transition in transitions"
      :key="transition.id"
      :class="transition.amount > 0 ? 'plus ' : 'minus'" >
      {{ transition.text }} <span>₹{{ transition.amount }}</span
      ><button @click="deletetransaction(transition.id )" class="delete-btn">x</button>
    </li>
  </ul>
  <props/>
</template>

<script setup>

import { defineProps } from 'vue';
const emit = defineEmits(['transactionDeleted']) 

const props = defineProps({
  transitions: {
    type: Array,
    required: true,
  },
});

const deletetransaction = (id) =>{
  emit('transactionDeleted', id )
}
</script>
