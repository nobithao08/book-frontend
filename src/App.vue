<script>
import AppHeader from "@/components/AppHeader.vue";
export default {
  components: {
    AppHeader,
  },
};
</script>

<template>
  <div id="container">
    <AppHeader />
    <div class="container" id="body">
      <router-view />
    </div>
  </div>
</template>

<style scoped>
/* #container {
  background-image: url("assets/bg2.jpg");
  background-size: 100% 100%;
  background-attachment: fixed;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
} */
#body{
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

</style>