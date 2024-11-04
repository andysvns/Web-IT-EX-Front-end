<template>
  <v-app>
    <Navbar />
    <router-view> </router-view>
    <Footer />
  </v-app>
  <!-- <v-app>
  <div id="app">
    <Navbar />
    <v-main>
      <router-view></router-view>
    </v-main>
    <Footer />
  </div>
</v-app> -->
</template>

<script>
import Footer from "./components/Footer.vue";
import Navbar from "./components/Navbar.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Footer,
  },
};
</script>
<style>
.background-b {
  background-image: url("/src/assets/bg2.png");
  background-size: cover;
  background-position: center;
  height: 450px;
  align-items: center;
}
.footer {
  background-image: url("/src/assets/bg2.png");
  background-size: cover;
  background-position: center;
}
@media (max-width: 768px) {
  .background-b {
    height: 250px;
  }
}
</style>
