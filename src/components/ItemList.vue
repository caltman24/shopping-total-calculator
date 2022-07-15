<template>
  <main>
    <div class="item-list-container">
      <div class="item-list">
        <div class="item-row" v-for="item in items" :key="item.id">
          <div class="item-desc">
            <p class="item-desc-name">{{ item.name || "" }}</p>
            <h2 class="item-desc-price">${{ item.price.toFixed(2) }}</h2>
          </div>
          <h2 class="item-acc">$0.00</h2>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "ItemList",
  props: {
    items: Array,
  },
};
</script>

<style scoped></style>
