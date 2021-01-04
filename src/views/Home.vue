<template>
  <div class="home">
    <Logo />
    <!-- <Header title="libraries" /> -->
    <Library
      v-for="i in Object.keys(libraries)"
      :key="i"
      :library="libraries[i]"
      :name="i"
    />
  </div>
</template>

<script>
import omodei from "omodei";

export default {
  name: "Home",
  data: () => ({
    libraries: {},
  }),
  components: {
    Header: require("../components/Header.vue").default,
    Logo: require("../components/Logo.vue").default,
    Library: require("../components/Library.vue").default,
  },

  mounted() {
    omodei.assignAll();
    this.libraries = omodei.getAll();
  },
};
</script>

<style>
.home {
  /* background: var(--back); */
  padding: 20px;
  width: 720px;
  border-radius: 8px;
  margin: 30px 0px;
  text-transform: uppercase;
  letter-spacing: 0.25ch;
}
</style>
