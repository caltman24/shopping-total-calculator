<template>
  <footer>
    <div class="total-result">
      <div class="total-row">
        <div class="total-sub">
          subtotal:
          <h3>${{ subtotal.toFixed(2) }}</h3>
        </div>
        <div class="total-tax">
          tax:
          <h3>${{ totalTax.toFixed(2) }}</h3>
        </div>
      </div>
      <div class="total-dmg">
        Total Damage:
        <h3>${{ totalPrice }}</h3>
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
