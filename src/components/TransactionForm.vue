<template>
  <form>
    <input
      type="text"
      id="description"
      placeholder="Descrição"
      required
      :value="descriptionModel"
      @input="$emit('update:descriptionModel', $event.target.value)"
    />
    <input
      type="number"
      id="value"
      placeholder="Valor (R$)"
      required
      :value="valueModel"
      @input="$emit('update:valueModel', $event.target.value)"
    />

    <div class="radio-container">
      <div class="radio-input-container">
        <input
          value="Receita"
          @input="$emit('update:typeModel', $event.target.value)"
          type="radio"
          name="type"
          id="receita"
        />
        <label for="receita">Receita</label>
      </div>

      <div class="radio-input-container">
        <input
          value="Despesa"
          @input="$emit('update:typeModel', $event.target.value)"
          type="radio"
          name="type"
          id="despesa"
        />
        <label for="despesa">Despesa</label>
      </div>
    </div>

    <button type="submit" @click.prevent="$emit('add')">Adicionar</button>
  </form>
</template>
<script>
export default {
  name: "TransactionForm",
  props: ["transactions", "descriptionModel", "valueModel", "typeModel"],
  emits: [
    "update:descriptionModel",
    "update:valueModel",
    "update:typeModel",
    "add",
  ],
};
</script>

<style lang="scss">
form {
  display: grid;
  grid-template-columns: auto auto auto auto;
  gap: 1.5rem;
  align-items: center;
  max-width: 100%;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0px 0px 10px #dcdce6;
  padding: 1rem;

  @media (max-width: 500px) {
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }

  [type="text"],
  [type="number"] {
    letter-spacing: -0.5px;
    border: 1px solid #c5c5c5;
    border-radius: 6px;
    padding: 8px 16px;

    &:focus {
      border: 1px solid #333;
      box-shadow: 0px 0px 3px #a5a5a5;
    }

    @media (max-width: 500px) {
      &:nth-child(1) {
        grid-column: 1/3;
        grid-row: 1;
      }

      &:nth-child(2) {
        grid-column: 1/3;
        grid-row: 2;
      }
    }
  }

  .radio-container {
    display: flex;
    gap: 10px;

    .radio-input-container {
      display: flex;
      gap: 5px;

      input {
        width: 20px;
        accent-color: #333;
      }
    }
  }

  button {
    height: 100%;
    background-color: #333;
    color: #fff;
    border-radius: 6px;
    padding: 8px 16px;
    cursor: pointer;
    transition: 0.3s;

    &:hover {
      box-shadow: 0px 0px 10px #a5a5a5;
    }

    @media (max-width: 500px) {
      grid-column: 2;
      grid-row: 3;
    }
  }
}
</style>
