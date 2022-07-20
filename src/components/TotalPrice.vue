<template>
  <footer>
    <div class="total-result">
      <div class="total-row">
        <div class="total-sub">
          <p>subtotal:</p>
          <h3>${{ subtotal.toFixed(2) }}</h3>
        </div>
        <div class="total-tax">
          <p>tax:</p>
          <h3>${{ totalTax.toFixed(2) }}</h3>
        </div>
        <div class="total-dmg">
          <p>Total Damage:</p>
          <h3>${{ totalPrice }}</h3>
        </div>
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

<style></style>
