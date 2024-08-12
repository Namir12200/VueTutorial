<template>
  <Header />
  <div class="container">
  <Balance :total="total"/>
  <IncomeExpense :income="income" :expense="expense" />
  <TransactionList :transactions="transactions" @deleteTransaction="handleTransactionDeleted" />
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { ref, computed } from 'vue';

  const transactions = ref([
    { id: 1, text: 'Flower', amount: -19.99 },
    { id: 2, text: 'Salary', amount: 299.97 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 150 }
  ]);

  const maxId = ref(4);

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0);
  })

  const income = computed(() => {
    return transactions.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

  const expense = computed(() => {
    return transactions.value.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

  const handleTransactionSubmitted = (transactionData) => {
    
    transactions.value.push({
      id: maxId.value++,
      text: transactionData.text,
      amount: transactionData.amount
    })
  }

  const handleTransactionDeleted = (transactionId) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id != transactionId)
  }

</script>

<!-- <style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style> -->
