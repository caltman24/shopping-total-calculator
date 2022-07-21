<template>
  <div class="item-row">
    <div class="item-desc" :class="item.isFood ? 'food' : ''">
      <p class="item-desc-name">
        {{ item.name || "" }}
        {{ unitWeight }}
        {{ unitPrice }}
      </p>
      <h4 class="item-desc-price">+ ${{ item.price.toFixed(2) }}</h4>
    </div>
    <p v-show="item.quantity > 1">{{quantity}}</p>
    <h4 class="item-acc">${{ accumPrice.toFixed(2) }}</h4>
  </div>
</template>

<script>
export default {
  name: "ItemRow",
  props: {
    item: Object,
    accumPrice: Number,
  },
  computed: {
    unitWeight() {
      return this.item.unitWeight
        ? `~ ${this.item.unitWeight} ${this.item.unit}`
        : "";
    },
    unitPrice() {
      return this.item.unitPrice
        ? `@  $${this.item.unitPrice}/${this.item.unit}`
        : "";
    },
    quantity() {
      return this.item.quantity
      ? `x ${this.item.quantity} @ $${(this.item.price / this.item.quantity).toFixed(2)}`
      : "";
    },
  },
};
</script>

<style>
h4 span {
  font-weight: 100;
}
</style>
