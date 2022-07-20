<template>
  <h1 id="title">Shopping total calculator</h1>
  <router-view v-slot="{ Component }">
    <transition name="slide" mode="out-in">
      <keep-alive :exclude="AboutPage">
        <component :is="Component" :key="$route.path"/>
      </keep-alive>
    </transition>
  </router-view>
</template>

<script>
export default {
  name: "App",
};
</script>
