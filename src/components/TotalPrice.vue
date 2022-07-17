<template>
  <footer>
    <div class="total-result">
      <div class="total-row">
        <div class="total-sub">
          subtotal:
          <h2>${{ subtotal.toFixed(2) }}</h2>
        </div>
        <div class="total-tax">
          tax:
          <h2>${{ totalTax.toFixed(2) }}</h2>
        </div>
      </div>
      <div class="total-dmg">
        Total Damage:
        <h2>${{ totalPrice }}</h2>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: "TotalPrice",
  props: {
    totalPrice: String,
    subtotal: Number,
    totalTax: Number,
  },
};
</script>
