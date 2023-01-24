<template>
  <div class="content">
    <h1>Controle financeiro</h1>

    <div class="infos">
      <ValueCard type="receitas">
        <template #type>
          <h2>Receitas</h2>
        </template>

        <template #typeIcon>
          <img src="./assets/icons/revenue.svg" alt="" />
        </template>

        <template #value>
          <p>{{ transformToCurrency(totalReceitas) }}</p>
        </template>
      </ValueCard>

      <ValueCard type="despesas">
        <template #type>
          <h2>Despesas</h2>
        </template>

        <template #typeIcon>
          <img src="./assets/icons/expense.svg" alt="" />
        </template>

        <template #value>
          <p>{{ transformToCurrency(totalDespesas) }}</p>
        </template>
      </ValueCard>

      <ValueCard type="total">
        <template #type>
          <h2>Total</h2>
        </template>

        <template #typeIcon>
          <img src="./assets/icons/total.svg" alt="" />
        </template>

        <template #value>
          <p>{{ transformToCurrency(totalSaldo) }}</p>
        </template>
      </ValueCard>
    </div>
    <TransactionForm
      @add="addTransaction"
      :transactions="transactions"
      v-model:description-model="description"
      v-model:type-model="type"
      v-model:value-model="value"
    />
    <TransactionsList
      @delete="deleteTransaction"
      :transactions="transactions"
    />
  </div>
</template>

<script>
import ValueCard from "@/components/ValueCard.vue";
import TransactionForm from "@/components/TransactionForm.vue";
import TransactionsList from "@/components/TransactionsList.vue";

export default {
  name: "App",
  components: {
    ValueCard,
    TransactionForm,
    TransactionsList,
  },

  data() {
    return {
      description: "",
      type: "",
      value: "",

      transactions: [],
      total: 0,
    };
  },

  computed: {
    totalSaldo() {
      return this.totalReceitas - this.totalDespesas;
    },

    totalReceitas() {
      return this.calcTotal("Receita");
    },

    totalDespesas() {
      return this.calcTotal("Despesa");
    },
  },

  created() {
    if (localStorage.transactions) {
      this.transactions = JSON.parse(localStorage.transactions);
    }
  },

  methods: {
    calcTotal(type) {
      return this.transactions
        .filter((transaction) => transaction.type === type)
        .reduce((acc, { value }) => {
          return +acc + +value;
        }, 0);
    },

    transformToCurrency(value) {
      return value.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
      });
    },

    addTransaction() {
      if (this.description && this.type && this.value) {
        const date = new Date();

        this.transactions.unshift({
          description: this.description,
          value: this.value,
          currency: this.transformToCurrency(+this.value),
          type: this.type,

          date: `${date.toLocaleDateString("pt-BR")}, ${date
            .toLocaleTimeString("pt-BR")
            .slice(0, 5)}`,
        });

        localStorage.transactions = JSON.stringify(this.transactions);

        this.description = "";
        this.value = "";
      }
    },

    deleteTransaction(transaction) {
      const transactionIndex = this.transactions.indexOf(transaction);
      this.transactions.splice(transactionIndex, 1);
      localStorage.transactions = JSON.stringify(this.transactions);
    },
  },
};
</script>

<style lang="scss">
@import "./scss/base/reset.scss";

body {
  width: 100vw;
  height: 100vh;
  letter-spacing: -0.5px;
  color: #111;
  background: #ebebf4;
}

h1 {
  font-size: 2.7rem;
  font-weight: 600;
  text-align: center;
  letter-spacing: -1px;
  margin-top: 30px;
  margin-bottom: 30px;

  @media (max-width: 1000px) {
    font-size: 1.8rem;
    margin-top: 3rem;
    margin-bottom: 15px;
  }
}

.content {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 950px;
  padding: 20px;
  margin: 0 auto;
  overflow-x: scroll;
}

.infos {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
  max-width: 100%;
  @media (max-width: 1000px) {
    display: flex;
    flex-direction: column;
  }
}
</style>
