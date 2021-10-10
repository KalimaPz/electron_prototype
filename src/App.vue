<template>
  <v-app>
    <v-toolbar
      dark
      floating
      color="gold"
      extended
      style="display: flex; align-items: center"
    >
      <v-toolbar-title>SGJ Delivery</v-toolbar-title>
    </v-toolbar>

    <v-main> <CreateOrder /> </v-main>
  </v-app>
</template>

<script>
import CreateOrder from "./components/order_create.vue";
export default {
  name: "App",

  components: {
    CreateOrder,
  },

  data: () => ({
    //
  }),
};
</script>
<style scoped>
.header {
  display: flex;
  background-color: beige;
  justify-content: center;
}
</style>
