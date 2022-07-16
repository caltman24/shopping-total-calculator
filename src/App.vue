<template>
  <h1 id="title">Shopping total calculator</h1>
  <HeaderToggle
    @toggle-menu="toggleMenu"
    @new-item="addItem"
    @clear-items="clearItems"
    :menuOpen="menuOpen"
    :items="items"
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
      items: [],
      taxAmount: 0.07,
    };
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
    },
    addItem(newItem) {
      this.items = [...this.items, newItem];
      localStorage.setItem("items", JSON.stringify(this.items));
      setTimeout(() => {
        this.setScroll();
      }, 100);
    },
    clearItems() {
      this.items = [];
      localStorage.setItem("items", JSON.stringify(this.items));
      setTimeout(() => {
        this.setScroll();
      }, 100);
    },
    setScroll() {
      const lastItem = document.querySelector(".item-row:last-child");
      const listContainer = document.querySelector(".item-list-container");
      listContainer.scrollTo(0, lastItem?.offsetTop);
    },
  },
  mounted() {
    this.setScroll();
    if (localStorage.items) {
      this.items = JSON.parse(localStorage.items);
    } else {
      localStorage.items = JSON.stringify(this?.items);
    }
  },
  computed: {
    totalPrice() {
      const subTotal = this.items.reduce((acc, curr) => acc + curr.price, 0);
      const tax = subTotal * this.taxAmount;
      const total = subTotal + tax;
      return total.toFixed(2);
    },
  },
  components: { HeaderToggle, ItemList },
};
</script>
