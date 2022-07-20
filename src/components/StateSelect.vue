<template>
  STATE:
  <select
    name="state-select"
    id="state-select"
    @change="selectState"
    v-model="state"
  >
    <option v-for="(state, i) in states" :key="i">
      {{ state }}
    </option>
  </select>
</template>

<script>
export default {
  name: "StateSelect",
  props: {
    states: Array,
    selectedState: String,
  },
  data() {
    return {
      state: this.selectedState,
    };
  },
  emits: ["state-select"],
  methods: {
    selectState(e) {
      this.state = e.target.value;
      this.$emit("state-select", this.state);
    },
  },
};
</script>
