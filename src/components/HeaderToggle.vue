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
      <AddItem v-show="menuOpen" @new-item="newItem" />
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
import AddItem from "./AddItem.vue";

export default {
  name: "HeaderToggle",
  props: {
    menuOpen: Boolean,
    items: Array,
  },
  emits: ["toggle-menu", "new-item", "clear-items"],
  methods: {
    toggleMenu() {
      this.$emit("toggle-menu");
    },
    newItem(item) {
      this.$emit("new-item", item);
    },
  },
  components: { AddItem },
};
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  width: 100%;
}
</style>
