<template>
  <div id="home">
    <StateSelect
      :states="states"
      @state-select="selectState"
      :selectedState="selectedState"
      :taxPercentage="taxPercentage"
    />
    <HeaderToggle
      @toggle-menu="toggleMenu"
      @new-item="addItem"
      @clear-items="clearItems"
      :menuOpen="menuOpen"
      :items="items"
    />
    <ItemList :items="items" @remove-item="deleteItem" />
    <router-link to="/about">ABOUT</router-link>
    <Transition name="fade">
      <TotalPrice
        :totalPrice="totalPrice"
        :totalTax="totalTax"
        :subtotal="subtotal"
        v-show="items.length > 0"
      />
    </Transition>
  </div>
</template>

<script>
import HeaderToggle from "@/components/HeaderToggle.vue";
import StateSelect from "@/components/StateSelect.vue";
import ItemList from "@/components/Item-List/ItemList.vue";
import TotalPrice from "@/components/TotalPrice.vue";

export default {
  name: "HomePage",
  data() {
    return {
      menuOpen: false,
      totalAmmount: 0,
      items: [],
      states: [],
      taxAmount: 0.07,
      selectedState: "IN",
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
    deleteItem(id) {
      this.items = this.items.filter((item) => item.id !== id);
      localStorage.setItem("items", JSON.stringify(this.items));
      setTimeout(() => {
        this.setScroll();
      }, 100);
    },
    clearItems() {
      const clearItemsCallback = () => {
        if (!confirm("Are you sure you want to clear all items?")) return;
        this.items = [];
        localStorage.setItem("items", JSON.stringify(this.items));
        setTimeout(() => {
          this.setScroll();
        }, 100);
      };
      // This is a hack to make the btn active transition to work. Else the confirmation will prevent the transition from happening.
      setTimeout(clearItemsCallback, 50);
    },
    setScroll() {
      const lastItem = document.querySelector(".item-row:last-child");
      if (!lastItem) return;
      const listContainer = document.querySelector(".item-list-container");
      listContainer.scrollTo(0, lastItem?.offsetTop);
    },
    selectState(state) {
      this.selectedState = state;
      this.setTax();
    },
    async setTax() {
      const tax = await this.getTaxByState(this.selectedState);
      localStorage.setItem("selectedState", this.selectedState);
      localStorage.setItem("tax", tax);
      this.taxAmount = tax;
    },
    async getStates() {
      const res = await fetch("./state_taxes.json");
      const data = await res.json();
      return Object.keys(data);
    },
    async getTaxByState(state) {
      const res = await fetch("./state_taxes.json");
      const taxes = await res.json();
      if (!taxes[state]) throw new Error("Invalid state");
      return taxes[state];
    },
  },
  created() {
    // pull items from local storage
    localStorage.getItem("items")
      ? (this.items = JSON.parse(localStorage.getItem("items")))
      : localStorage.setItem("items", JSON.stringify(this?.items));

    // pull selected state from local storage
    localStorage.getItem("selectedState")
      ? (this.selectedState = localStorage.getItem("selectedState"))
      : localStorage.setItem("selectedState", this.selectedState);

    // pull tax from local storage
    localStorage.getItem("tax")
      ? (this.taxAmount = localStorage.getItem("tax"))
      : localStorage.setItem("tax", this.taxAmount);

    // pull states from json file
    this.getStates().then((states) => {
      this.states = states;
    });
  },
  mounted() {
    setTimeout(() => {
      this.setScroll();
    }, 50);
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
    taxPercentage() {
      return (this.taxAmount * 100).toFixed(2);
    },
    totalPrice() {
      const total = this.subtotal + this.totalTax;
      return total.toFixed(2);
    },
  },
  components: { HeaderToggle, ItemList, TotalPrice, StateSelect },
};
</script>

<style scoped>
#home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
