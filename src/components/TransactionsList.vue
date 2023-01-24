<template lang="">
  <table class="container" v-if="transactions.length">
    <thead class="types-list">
      <tr>
        <th><h3 class="type">Descrição</h3></th>
        <th><h3 class="type">Valor</h3></th>
        <th><h3 class="type">Data</h3></th>
        <th><h3 class="type">Tipo</h3></th>
      </tr>
    </thead>

    <tbody class="transaction">
      <tr v-for="transaction in transactions" :key="transaction.description">
        <td class="transaction-info">{{ transaction.description }}</td>
        <td class="transaction-info">{{ transaction.currency }}</td>
        <td class="transaction-info">{{ transaction.date }}</td>
        <td class="transaction-info">
          <img
            v-if="transaction.type === 'Receita'"
            src="@/assets/icons/type_revenue.svg"
            alt="Receita"
          />

          <img v-else src="@/assets/icons/type_expense.svg" alt="Despesa" />
        </td>

        <td class="trash" @click.prevent="$emit('delete', transaction)">
          <img src="@/assets/icons/trash.svg" alt="Excluir" />
        </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name: "TransactionsList",
  props: ["transactions"],
  emits: ["delete"],
};
</script>

<style lang="scss">
table {
  position: relative;
  max-width: 100vw;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0px 0px 10px #dcdce6;
  padding: 1rem;
  overflow-x: scroll;

  @media (max-width: 1000px) {
    display: block;
  }
}

thead {
  padding-top: 1rem;

  tr {
    th {
      text-align: start;

      @media (max-width: 1000px) {
        padding-right: 3rem;
      }

      &:nth-child(5) {
        h3 {
          color: transparent;
        }
      }

      h3 {
        font-weight: 600;
        padding-bottom: 1rem;
      }
    }
  }
}

tbody {
  tr:last-child td {
    padding-bottom: 0;
  }

  td {
    padding: 0.5rem 0;

    &.trash {
      cursor: pointer;
    }

    @media (max-width: 1000px) {
      padding: 0.5rem 3rem 0.5rem 0;

      &:nth-child(5) {
        padding: 0.5rem 0 0.5rem 0;
      }
    }
  }
}
</style>
