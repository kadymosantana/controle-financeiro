<template>
  <div class="content">
    <h1>Controle financeiro</h1>

    <div class="infos">
      <ValueCard type="receitas">
        <template #type>
          <h2>Receitas</h2>
        </template>

        <template #typeIcon>
          <svg width="36" height="36" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M12 20c-4.41 0-8-3.59-8-8s3.59-8 8-8s8 3.59 8 8s-3.59 8-8 8m0 2c5.52 0 10-4.48 10-10S17.52 2 12 2S2 6.48 2 12s4.48 10 10 10zm-1-10v4h2v-4h3l-4-4l-4 4h3z"
            />
          </svg>
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
          <svg width="36" height="36" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M12 4c4.41 0 8 3.59 8 8s-3.59 8-8 8s-8-3.59-8-8s3.59-8 8-8m0-2C6.48 2 2 6.48 2 12s4.48 10 10 10s10-4.48 10-10S17.52 2 12 2zm1 10V8h-2v4H8l4 4l4-4h-3z"
            />
          </svg>
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
          <svg width="36" height="36" viewBox="0 0 24 24">
            <path
              fill="currentColor"
              d="M11.8 10.9c-2.27-.59-3-1.2-3-2.15c0-1.09 1.01-1.85 2.7-1.85c1.42 0 2.13.54 2.39 1.4c.12.4.45.7.87.7h.3c.66 0 1.13-.65.9-1.27c-.42-1.18-1.4-2.16-2.96-2.54V4.5c0-.83-.67-1.5-1.5-1.5S10 3.67 10 4.5v.66c-1.94.42-3.5 1.68-3.5 3.61c0 2.31 1.91 3.46 4.7 4.13c2.5.6 3 1.48 3 2.41c0 .69-.49 1.79-2.7 1.79c-1.65 0-2.5-.59-2.83-1.43c-.15-.39-.49-.67-.9-.67h-.28c-.67 0-1.14.68-.89 1.3c.57 1.39 1.9 2.21 3.4 2.53v.67c0 .83.67 1.5 1.5 1.5s1.5-.67 1.5-1.5v-.65c1.95-.37 3.5-1.5 3.5-3.55c0-2.84-2.43-3.81-4.7-4.4z"
            />
          </svg>
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
