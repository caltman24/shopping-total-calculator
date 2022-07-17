<template>
  <div class="item-row">
    <div class="item-desc" :class="item.isFood ? 'food' : ''">
      <p class="item-desc-name">{{ item.name || "" }}</p>
      <h3 class="item-desc-price">+ ${{ item.price.toFixed(2) }}</h3>
    </div>
    <h3 class="item-acc">${{ accumPrice.toFixed(2) }}</h3>
  </div>
</template>

<script>
export default {
  name: "ItemRow",
  props: {
    item: Object,
    accumPrice: Number,
  },
};
</script>
