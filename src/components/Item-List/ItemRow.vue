<template>
  <div class="item-row">
    <div class="item-desc" :class="item.isFood ? 'food' : ''">
      <p class="item-desc-name">{{ item.name || "" }} {{ unitWeight }}</p>
      <h4 class="item-desc-price">
        + ${{ item.price.toFixed(2) }} <span>{{ pricePerWeight }}</span>
      </h4>
    </div>
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
    pricePerWeight() {
      return this.item.unitWeight
        ? `~ $${(this.item.price / this.item.unitWeight).toFixed(2)}/${
            this.item.unit
          }`
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
