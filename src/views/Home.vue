<template>
  <div class="home d-flex align-center justify-center fill-height">
    <div class="d-flex flex-column align-center">
      <RandomBasket max-count="36" />

      <v-btn color="primary" class="mt-10" id="random-btn">抽籤</v-btn>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import RandomBasket from "@/components/RandomBasket.vue";

@Component({
  components: {
    RandomBasket
  }
})
export default class Home extends Vue {}
</script>

<style lang="scss" scoped>
#random-btn {
  height: 8vh;
  min-width: 10vw;
  padding: 0 1.25em;
  font-size: 5vh;
  border-radius: 4vh;
}
</style>
