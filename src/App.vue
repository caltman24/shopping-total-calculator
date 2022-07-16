<template>
  <h1 id="title">Shopping total calculator</h1>
  <HeaderToggle
    @toggle-menu="toggleMenu"
    @new-item="addItem"
    @clear-items="clearItems"
    :menuOpen="menuOpen"
    :items="items"
  />
  <ItemList :items="items" @remove-item="deleteItem" />
  <Transition name="fade">
    <TotalPrice
      :totalPrice="totalPrice"
      :totalTax="totalTax"
      :subtotal="subtotal"
      v-show="items.length > 0"
    />
  </Transition>
</template>

<script>
import HeaderToggle from "./components/HeaderToggle.vue";
import ItemList from "./components/ItemList.vue";
import TotalPrice from "./components/TotalPrice.vue";
import { v4 as uuidv4 } from "uuid";

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
      const itemWithId = {
        id: uuidv4(),
        ...newItem,
      };
      this.items = [...this.items, itemWithId];
      localStorage.setItem("items", JSON.stringify(this.items));
      setTimeout(() => {
        this.setScroll();
      }, 100);
    },
    deleteItem(id) {
      this.items = this.items.filter((item) => item.id !== id);
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
    if (localStorage.items) {
      this.items = JSON.parse(localStorage.items);
    } else {
      localStorage.items = JSON.stringify(this?.items);
    }
    setTimeout(() => {
      this.setScroll();
    }, 1);
  },
  computed: {
    subtotal() {
      return this.items.reduce((acc, item) => {
        return acc + item.price;
      }, 0);
    },
    totalTax() {
      const nonFoods = this.items.filter((item) => !item.isFood);
      const nonFoodTotal = nonFoods.reduce((acc, curr) => acc + curr.price, 0);

      const tax = nonFoodTotal * this.taxAmount;
      return tax;
    },
    totalPrice() {
      return (this.subtotal + this.totalTax).toFixed(2);
    },
  },
  components: { HeaderToggle, ItemList, TotalPrice },
};
</script>
