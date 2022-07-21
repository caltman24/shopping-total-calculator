<template>
  <form @submit.prevent="saveItem" class="add-item">
    <div class="options-container">
      <div class="price-type-toggle">
        <p v-if="selectedPriceField === 'fixed'">Fixed Price</p>
        <p v-else>Price Per Unit</p>
        <SwitchToggle @toggle-switch="togglePriceField" />
      </div>
      <span>
        <input type="checkbox" name="" id="is-food" v-model="isFood" />
        <label for="is-food">Food</label>
      </span>
    </div>
    <div class="item-name--wrapper">
      <label for="item-name">Name: </label>
      <input id="item-name" type="text" placeholder="Bread" v-model="name" />
    </div>
    <transition name="form-fade" mode="out-in">
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
        <label for="item-qty">Qty: </label>
        <input
          id="item-qty"
          type="number"
          min="1"
          step="1"
          v-model.number="quantity"
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
    </transition>

    <button type="submit" class="btn">Save Item</button>
  </form>
</template>

<script>
import SwitchToggle from "./SwitchToggle.vue";
import { v4 as uuidv4 } from "uuid";

export default {
  name: "AddItem",
  data() {
    return {
      name: "",
      price: null,
      unitPrice: null,
      unitWeight: null,
      unit: "lbs",
      isFood: true,
      selectedPriceField: "fixed",
      quantity: 1,
    };
  },
  components: {
    SwitchToggle,
  },
  methods: {
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
          unitPrice: this.unitPrice,
          unit: this.unit,
        };
      } else if (this.selectedPriceField === "fixed") {
        this.price = this.price * this.quantity;
        newItem = {
          id: uuidv4(),
          name: this.name,
          price: this.price,
          isFood: this.isFood,
          quantity: this.quantity,
        };
      } else throw new Error("selectedPriceField is invalid");
      this.$emit("new-item", newItem);
      this.clearForm();
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
      this.quantity = 1;
    },
  },
  emits: ["new-item"],
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

input {
  padding: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-block: 0.35em;
  font-size: 1rem;
  width: 100%;
}

.item-name--wrapper {
  width: 100%;
}

.item-price--wrapper {
  width: 100%;
}

.price-type-toggle {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 0.5em;
}

.price-type-toggle p {
  text-align: right;
}

.options-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 4em;
  margin-top: 0.5em;

  width: 100%;
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
</style>
