<template>
  <div>
    <input
      type="text"
      class="form-control bg-dark text-light rounded-0 border-0 my-4"
      placeholder="Search Coin"
      @keyup="searchCoin"
    />
  </div>
</template>

<script>
export default {
  methods: {
    searchCoin(event) {
      this.$emit("search", event.target.value);
    },
  },
};
</script>