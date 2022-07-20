<template>
  <header>
    <button
      @click.prevent="toggleMenu"
      type="button"
      class="btn large"
      :class="menuOpen ? 'close' : ''"
    >
      {{ menuOpen ? "Close X" : "Add Item +" }}
    </button>
    <transition name="fade">
      <form @submit.prevent="saveItem" class="add-item" v-show="menuOpen">
        <button type="button" @click.prevent="togglePriceField" class="btn toggle">
          Toggle Unit Pricing
        </button>
        <div class="item-name--wrapper">
          <label for="item-name">Name: </label>
          <input
            id="item-name"
            type="text"
            placeholder="Bread"
            v-model="name"
          />
        </div>
        <div
          class="item-price--wrapper fixed"
          v-if="selectedPriceField === 'fixed'"
        >
          <label for="item-price">Price: </label>
          <input
            id="item-price"
            type="number"
            min="0.01"
            step="0.01"
            placeholder="0.00"
            v-model.number="price"
            required
          />
        </div>
        <div
          class="item-price--wrapper unit"
          v-else-if="selectedPriceField === 'unit'"
        >
          <label for="item-price">Unit Price: </label>
          <input
            id="item-price"
            type="number"
            min="0.01"
            step="0.01"
            placeholder="0.00"
            v-model.number="unitPrice"
            required
          />
          <label for="item-weight">Weight: </label>
          <input
            id="item-weight"
            type="number"
            min="0.01"
            step="0.01"
            placeholder="0.00"
            v-model.number="unitWeight"
            required
          />
          <select v-model="unit">
            <option value="lbs">lbs</option>
            <option value="oz">oz</option>
            <option value="g">g</option>
            <option value="kg">kg</option>
          </select>
        </div>

        <span>
          <input type="checkbox" name="" id="is-food" v-model="isFood" />
          <label for="is-food">Food</label>
        </span>

        <button type="submit" class="btn">Save Item</button>
      </form>
    </transition>
    <transition name="fade">
      <button
        v-show="items.length > 0 && !menuOpen"
        @click.prevent="clearItems"
        type="button"
        class="btn clear large"
      >
        clear
      </button>
    </transition>
  </header>
</template>

<script>
import { v4 as uuidv4 } from "uuid";

export default {
  name: "HeaderToggle",
  props: {
    menuOpen: Boolean,
    items: Array,
  },
  data() {
    return {
      name: "",
      price: null,
      unitPrice: null,
      unitWeight: null,
      unit: "lbs",
      isFood: true,
      selectedPriceField: "fixed",
    };
  },
  emits: ["toggle-menu", "new-item", "clear-items"],
  methods: {
    toggleMenu() {
      this.$emit("toggle-menu");
    },
    saveItem() {
      if (!this.price && this.selectedPriceField === "fixed") return;
      if (
        !this.unitPrice &&
        !this.unitWeight &&
        this.selectedPriceField === "unit"
      ) {
        return;
      }

      let newItem = {};
      if (this.selectedPriceField === "unit") {
        this.price = this.unitPrice * this.unitWeight;
        newItem = {
          id: uuidv4(),
          name: this.name,
          price: this.price,
          isFood: this.isFood,
          unitWeight: this.unitWeight,
          unit: this.unit,
        };
      } else if (this.selectedPriceField === "fixed") {
        newItem = {
          id: uuidv4(),
          name: this.name,
          price: this.price,
          isFood: this.isFood,
        };
      } else throw new Error("selectedPriceField is invalid");

      this.$emit("new-item", newItem);
      this.clearForm();
    },
    clearItems() {
      this.$emit("clear-items");
    },
    togglePriceField() {
      this.selectedPriceField =
        this.selectedPriceField === "fixed" ? "unit" : "fixed";
      this.clearForm();
    },
    clearForm() {
      this.name = "";
      this.price = null;
      this.unitPrice = null;
      this.unitWeight = null;
      this.unit = "lbs";
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  width: 100%;
}

form {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

input {
  padding: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-block: 0.25em;
  font-size: 1rem;
  width: 100%;
}

div.unit input {
  margin-right: 1.5em;
}

#item-weight {
  width: fit-content;
}

span {
  display: flex;
  align-items: center;
}

select {
  padding: 0.25em;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
}

#is-food {
  width: 20px;
  height: 20px;
  margin-right: 1em;
}

label {
  font-size: 1.1rem;
}
</style>
