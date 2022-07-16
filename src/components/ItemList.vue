<template>
  <main>
    <div class="fade-item-list top"></div>
    <div class="item-list-container">
      <div class="item-list">
        <transition-group name="list">
          <div
            class="item-row"
            v-for="(item, i) in items"
            :key="item.id"
            @dblclick="removeItem(item)"
          >
            <div class="item-desc" :class="item.isFood ? 'food' : ''">
              <p class="item-desc-name">{{ item.name || "" }}</p>
              <h3 class="item-desc-price">+ ${{ item.price.toFixed(2) }}</h3>
            </div>
            <h2 class="item-acc">${{ accumPrice[i].toFixed(2) }}</h2>
          </div>
        </transition-group>
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
  emits: ["remove-item"],
  methods: {
    removeItem(item) {
      this.$emit("remove-item", item.id);
    },
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

<style scoped>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-leave-active {
  opacity: 0;
  position: absolute;
}

.list-enter-from {
  opacity: 0;
}

.list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
