<template>
  <div id="home">
    <div class="content">
      <div class="state-select">
        <StateSelect
          :states="getStates()"
          @state-select="selectState"
          :selectedState="selectedState"
        />
        <strong class="blue">{{ taxPercentage }}%</strong>
      </div>
      <HeaderToggle
        @toggle-menu="toggleMenu"
        @new-item="addItem"
        @clear-items="clearItems"
        :menuOpen="menuOpen"
        :items="items"
      />
      <ItemList :items="items" @remove-item="deleteItem" />
      <router-link to="/about">ABOUT</router-link>
    </div>
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
      taxAmount: 0.07,
      selectedState: "IN",
      stateTaxes: {
        AL: "0.04",
        AK: "0.05",
        AZ: "0.056",
        AR: "0.065",
        CA: "0.0725",
        CO: "0.029",
        CT: "0.0635",
        DE: "0.00",
        DC: "0.06",
        FL: "0.06",
        GA: "0.04",
        HI: "0.04",
        ID: "0.06",
        IL: "0.0625",
        IN: "0.07",
        IA: "0.06",
        KS: "0.065",
        KY: "0.06",
        LA: "0.0445",
        ME: "0.055",
        MD: "0.06",
        MA: "0.0625",
        MI: "0.06",
        MN: "0.06875",
        MS: "0.07",
        MO: "0.04225",
        MT: "0.00",
        NE: "0.0550",
        NV: "0.0685",
        NH: "0.00",
        NJ: "0.06625",
        NM: "0.05",
        NY: "0.04",
        NC: "0.04750",
        ND: "0.05",
        OH: "0.0575",
        OK: "0.045",
        OR: "0.00",
        PA: "0.06",
        PR: "0.115",
        RI: "0.07",
        SC: "0.06",
        SD: "0.045",
        TN: "0.07",
        TX: "0.0625",
        UT: "0.0485",
        VT: "0.06",
        VA: "0.043",
        WA: "0.065",
        WV: "0.06",
        WI: "0.05",
        WY: "0.04",
      },
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
    setTax() {
      const tax = this.getTaxByState(this.selectedState);
      localStorage.setItem("selectedState", this.selectedState);
      localStorage.setItem("tax", tax);
      this.taxAmount = tax;
    },
    getStates() {
      return Object.keys(this.stateTaxes);
    },
    getTaxByState(state) {
      const stateTax = this.stateTaxes[state];
      if (!stateTax) throw new Error("Invalid state");
      return stateTax;
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

<style>
.state-select {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5em;
}

a:not(.external-link) {
  color: rgb(87, 86, 86);
  text-decoration: none;
  border-bottom: 2px solid var(--clr-blue);
  transition: transform 0.3s ease-in-out;
  letter-spacing: 1px;
  font-size: 1.1rem;
}

a:hover {
  transform: translateY(1.5px);
  transition: transform 0.2s ease-in-out;
}

a:active {
  transform: translateY(-1px);
  transition: transform 0.2s ease-in-out;
}
</style>
