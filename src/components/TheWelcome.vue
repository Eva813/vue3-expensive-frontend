<script setup lang="ts">
// import WelcomeItem from './WelcomeItem.vue'
// import DocumentationIcon from './icons/IconDocumentation.vue'
// import ToolingIcon from './icons/IconTooling.vue'
// import EcosystemIcon from './icons/IconEcosystem.vue'
// import CommunityIcon from './icons/IconCommunity.vue'
// import SupportIcon from './icons/IconSupport.vue'
import { ref, onMounted, computed } from 'vue';
const name = ref('');
const price = ref('');
const income = ref(0);
const expense = ref(0);
const item_list = ref([]);

const totalBalance = computed(() => income.value + expense.value);

const addTransaction = async () => {
  const data = {
    name: name.value,
    price: parseFloat(price.value),
    type: parseFloat(price.value) > 0 ? 'income' : 'outcome',
  };

  item_list.value.push(data);
  name.value = '';
  price.value = '';

  try {
    const response = await fetch('http://127.0.0.1:5000/item/', {
      method: 'POST',
      body: JSON.stringify(data),
      headers: { 'Content-Type': 'application/json' },
    });
    const result = await response.json();
    console.log(result);
  } catch (error) {
    console.error('Error:', error);
  }
};

const removeItem = async (item) => {
  const index = item_list.value.indexOf(item);
  if (index > -1) {
    try {
      const response = await fetch(`http://127.0.0.1:5000/item/${item._id.$oid}`, {
        method: 'DELETE',
      });
      const result = await response.json();
      console.log(result);
    } catch (error) {
      console.error('Error:', error);
    }
    item_list.value.splice(index, 1);
    updateTotals();
  }
};

const updateTotals = () => {
  income.value = item_list.value
    .filter((item) => item.type === 'income')
    .reduce((sum, item) => sum + parseFloat(item.price), 0);
  expense.value = item_list.value
    .filter((item) => item.type === 'outcome')
    .reduce((sum, item) => sum + parseFloat(item.price), 0);
};

onMounted(async () => {
  try {
    const response = await fetch('http://127.0.0.1:5000/items/', { method: 'GET' });
    const result = await response.json();
    console.log(result);
    item_list.value = result;
    updateTotals();
  } catch (error) {
    console.error('Error:', error);
  }
});
</script>

<template>
  <div class="expensive-container">
    <h4>Your Balance</h4>
    <h1 id="balance">${{ totalBalance }}</h1>

    <div class="inc-exp-container">
      <div>
        <h4>Income</h4>
        <p id="money-plus" class="money plus">${{ income }}</p>
      </div>
      <div>
        <h4>Expense</h4>
        <p id="money-minus" class="money minus">${{ expense }}</p>
      </div>
    </div>

    <h3>History</h3>
    <ul id="list" class="list">
      <li :class="{ 'plus': item.price > 0, 'minus': item.price < 0 }" v-for="item in item_list" :key="item._id.$oid">
        {{ item.name }} <span>{{ item.price }}</span>
        <button class="delete-btn" @click="removeItem(item)">x</button>
      </li>
    </ul>

    <h3>Add new transaction</h3>
    <form id="form">
      <div class="form-control">
        <label for="text">Text</label>
        <input v-model="name" type="text" id="text" placeholder="Enter text...">
      </div>
      <div class="form-control">
        <label for="amount">Amount <br> (negative - expense, positive - income)</label>
        <input v-model="price" type="number" id="amount" placeholder="Enter amount...">
      </div>
      <button @click.prevent="addTransaction" class="btn">Add transaction</button>
    </form>
  </div>
</template>
<style scoped>
.expensive-container {
  width: 400px;
  margin: 0 auto;
}
</style>