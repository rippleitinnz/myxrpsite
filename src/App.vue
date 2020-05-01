<template>
  <div id="app">
    <RipHead />
    <RipBody />
    <RipFooter />
  </div>
</template>

<script>
import RipHead from "./components/RipHead";
import RipBody from "./components/RipBody";
import RipFooter from "./components/RipFooter";

export default {
  name: "App",
  components: {
    RipHead,
    RipBody,
    RipFooter
  }
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
  list-style: none;
  text-decoration: none;
      overflow-x: hidden;
    margin-right: calc(-1 * (100vw - 100%));
}

</style>
