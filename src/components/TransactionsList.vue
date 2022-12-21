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
          <svg
            v-if="transaction.type === 'Receita'"
            width="28"
            height="28"
            viewBox="0 0 24 24"
          >
            <path
              fill="#21cf21"
              d="M12 20c-4.41 0-8-3.59-8-8s3.59-8 8-8s8 3.59 8 8s-3.59 8-8 8m0 2c5.52 0 10-4.48 10-10S17.52 2 12 2S2 6.48 2 12s4.48 10 10 10zm-1-10v4h2v-4h3l-4-4l-4 4h3z"
            />
          </svg>

          <svg v-else width="28" height="28" viewBox="0 0 24 24">
            <path
              fill="#d72525"
              d="M12 4c4.41 0 8 3.59 8 8s-3.59 8-8 8s-8-3.59-8-8s3.59-8 8-8m0-2C6.48 2 2 6.48 2 12s4.48 10 10 10s10-4.48 10-10S17.52 2 12 2zm1 10V8h-2v4H8l4 4l4-4h-3z"
            />
          </svg>
        </td>

        <td class="trash" @click.prevent="$emit('delete', transaction)">
          <svg width="28" height="28" viewBox="0 0 24 24">
            <path
              fill="#d72525"
              d="M14.12 10.47L12 12.59l-2.13-2.12l-1.41 1.41L10.59 14l-2.12 2.12l1.41 1.41L12 15.41l2.12 2.12l1.41-1.41L13.41 14l2.12-2.12zM15.5 4l-1-1h-5l-1 1H5v2h14V4zM6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM8 9h8v10H8V9z"
            />
          </svg>
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
