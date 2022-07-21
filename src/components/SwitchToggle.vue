<template>
  <label class="switch">
    <input type="checkbox" @click="toggle" />
    <div class="slider round"></div>
  </label>
</template>

<script>
export default {
  name: "HeaderToggle",
  methods: {
    toggle() {
      this.$emit("toggle-switch");
    },
  },
};
</script>

<style scoped>
/* SLIDER TOGGLE */

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 28px;
}

.switch input {
  display: none;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 23px;
  width: 23px;
  left: 6px;
  bottom: 3px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: var(--clr-food);
}

input:focus + .slider {
  box-shadow: 0 0 1px #101010;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
