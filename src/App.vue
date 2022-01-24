<template>
  <h2>Three divs</h2>
  <Rotator>
    <div class="rotateItem">
      <h5>Hello 1</h5>
      <img src="https://source.unsplash.com/EbuaKnSm8Zw/800x533" alt="" />
    </div>
    <div class="rotateItem">
      <h5>Hello 2</h5>
      <img src="https://source.unsplash.com/mCg0ZgD7BgU/800x533" alt="" />
    </div>
    <div class="rotateItem">
      <h5>Hello 3</h5>
      <img src="https://source.unsplash.com/nvzvOPQW0gc/800x533" alt="" />
    </div>
  </Rotator>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Rotator from "./components/Rotator.vue";

export default defineComponent({
  name: "App",
  components: {
    Rotator,
  },
});
</script>

<style lang="scss">
body {
  margin: 0;
  font-family: "Roboto", sans-serif;
  font-size: 16px;
}

h1 {
  text-align: center;
  margin-bottom: 1.5em;
}

h2 {
  text-align: center;
  color: #555;
  margin-bottom: 0;
}

.rotateItem {
  width: 800px;
}
</style>
