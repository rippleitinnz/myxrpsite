<template>
  <div id="app">
    <RipHead/>

    <RipBody/>

    <RipFooter/>
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

