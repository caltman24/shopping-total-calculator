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
        />
        <button @click.prevent="saveItem" type="submit" class="btn">
          Save Item
        </button>
      </form>
    </transition>
  </header>
</template>

<script>
export default {
  name: "HeaderToggle",
  props: {
    menuOpen: Boolean,
  },
  data() {
    return {
      name: "",
      price: null,
    };
  },
  emits: ["toggle-menu", "new-item"],
  methods: {
    toggleMenu() {
      this.$emit("toggle-menu");
    },
    saveItem() {
      const newItem = {
        name: this.name,
        price: this.price,
      };
      this.$emit("new-item", newItem);
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-block: 1em;
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
  margin-block: 0.5em;
  font-size: 20px;
}

label {
  font-size: 20px;
}
</style>
