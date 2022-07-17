<template>
  <header>
    <button
      @click.prevent="toggleMenu"
      type="button"
      class="btn large"
      :class="menuOpen ? 'close' : ''"
    >
      {{ menuOpen ? "Close" : "Add Item" }}
    </button>
    <transition name="fade">
      <form class="add-item" v-show="menuOpen">
        <label for="item-name">Name: </label>
        <input id="item-name" type="text" placeholder="Bread" v-model="name" />
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
        <span>
          <input type="checkbox" name="" id="is-food" v-model="isFood" />
          <label for="is-food">Food</label>
        </span>

        <button @click.prevent="saveItem" type="submit" class="btn">
          Save Item
        </button>
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
      isFood: true,
    };
  },
  emits: ["toggle-menu", "new-item", "clear-items"],
  methods: {
    toggleMenu() {
      this.$emit("toggle-menu");
    },
    saveItem() {
      if (!this.price) return;
      const newItem = {
        id: uuidv4(),
        name: this.name,
        price: this.price,
        isFood: this.isFood,
      };
      this.$emit("new-item", newItem);
      this.name = "";
      this.price = null;
      this.isFood = true;
    },
    clearItems() {
      this.$emit("clear-items");
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
}

input {
  width: 280px;
  padding: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-block: 0.25em;
  font-size: 1rem;
}

span {
  display: flex;
  align-items: center;
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
