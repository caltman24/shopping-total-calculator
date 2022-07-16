<template>
  <main>
    <div class="fade-item-list top"></div>
    <div class="item-list-container">
      <div class="item-list">
        <div class="item-row" v-for="(item, i) in items" :key="item.id">
          <div class="item-desc">
            <p class="item-desc-name">{{ item.name || "" }}</p>
            <h3 class="item-desc-price">${{ item.price.toFixed(2) }}</h3>
          </div>
          <h2 class="item-acc">${{ accumPrice[i].toFixed(2) }}</h2>
        </div>
      </div>
    </div>
    <div class="fade-item-list bottom"></div>
  </main>
</template>

<script>
export default {
  name: "ItemList",
  props: {
    items: Array,
  },
  computed: {
    accumPrice() {
      const prices = this.items.map((item) => item.price);
      const totals = [];

      prices.reduce((acc, curr) => {
        acc += curr;
        totals.push(acc);
        return acc;
      }, 0);

      return totals;
    },
  },
};
</script>

<style scoped></style>
