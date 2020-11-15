<template>
  <div id="app">
    <HomePage />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HomePage from "./components/Home.vue";

@Component({
  components: {
    HomePage
  }
})
export default class App extends Vue {}
</script>

<style lang="scss">
@font-face {
  font-family: "Christmas";
  src: url("/font/geometos.ttf");
}

.material-icons {
  font-family: "Material Icons";
  font-weight: normal;
  font-style: normal; /* Preferred icon size */
  display: inline-block;
  line-height: 1;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  white-space: nowrap;
  direction: ltr;

  /* Support for all WebKit browsers. */
  -webkit-font-smoothing: antialiased;
  /* Support for Safari and Chrome. */
  text-rendering: optimizeLegibility;

  /* Support for Firefox. */
  -moz-osx-font-smoothing: grayscale;

  /* Support for IE. */
  font-feature-settings: "liga";
}
</style>
