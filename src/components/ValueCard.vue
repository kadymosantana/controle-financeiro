<template>
  <div class="card">
    <div :style="colorCard" class="type">
      <slot name="type"></slot>
      <slot name="typeIcon"></slot>
    </div>

    <slot name="value"></slot>
  </div>
</template>

<script>
export default {
  name: "ValueCard",
  props: ["type"],

  computed: {
    colorCard() {
      if (this.type === "receitas")
        return {
          background: "linear-gradient(to top right, #07a007, #07c707)",
        };
      else if (this.type === "despesas")
        return {
          background: "linear-gradient(to top right, #9a0606, #d72525)",
        };
      else return { background: "linear-gradient(to top right, #222, #555)" };
    },
  },
};
</script>

<style lang="scss">
.card {
  background-color: #fff;
  letter-spacing: -0.5px;
  border-radius: 12px;
  box-shadow: 0px 0px 10px #dcdce6;
  overflow: hidden;

  .type {
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
    color: #fff;
    padding: 1rem;

    h2 {
      font-weight: 400;
    }
  }

  p {
    font-size: 2rem;
    font-weight: 600;
    padding: 1rem;
  }
}
</style>
