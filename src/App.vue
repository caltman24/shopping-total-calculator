<template>
  <h1 id="title">Shopping total calculator</h1>
  <HeaderToggle
    @toggle-menu="toggleMenu"
    @new-item="addItem"
    :menuOpen="menuOpen"
  />
  <ItemList :items="items" />
  <footer>
    <div class="total-result">
      Total Damage:
      <h2>${{ totalPrice }}</h2>
    </div>
  </footer>
</template>

<script>
import HeaderToggle from "./components/HeaderToggle.vue";
import ItemList from "./components/ItemList.vue";
export default {
  name: "App",
  data() {
    return {
      menuOpen: false,
      totalAmmount: 0,
      items: [
        { name: "Bacon", price: 2.0, id: 1 },
        { name: "Milk", price: 1.99, id: 2 },
        { name: "Sauce", price: 3.29, id: 3 },
        { name: "Beef", price: 6.29, id: 4 },
        { name: "Cheese", price: 3.89, id: 5 },
        { name: "Detergent", price: 9.23, id: 6 },
      ],
    };
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
    },
    addItem(newItem) {
      this.items = [...this.items, newItem];
    },
  },
  computed: {
    totalPrice() {
      return this.items.reduce((acc, curr) => acc + curr.price, 0).toFixed(2);
    },
  },
  components: { HeaderToggle, ItemList },
};
</script>
