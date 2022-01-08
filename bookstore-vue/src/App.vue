<template>
  <div class="navbar"><Header /></div>
  <router-view id="body_pages" />
</template>

<script>
import Header from "@/components/header.vue";
export default {
  components: {
    Header,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: grid;
  grid-template-rows: 100px 1fr;
}
:root {
  --cback: white; /*#181a1b;*/
  --cfont: black; /*#a1aeb5;*/
  --cback2: #6200ee;
  color: var(--cfont);
  background-color: var(--cback);
}
.navbar {
  grid-row: 1/2;
  position: fixed;
  width: 100%;
  z-index: 10000;
}
#body_pages {
  grid-row: 2;
  margin-top: 10px;
}
</style>
