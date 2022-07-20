<template>
  <router-view v-slot="{ Component }">
    <transition name="slide" mode="out-in" appear>
      <keep-alive>
        <component :is="Component" :key="$route.path" />
      </keep-alive>
    </transition>
  </router-view>
</template>

<script>
export default {
  name: "App",
};
</script>
