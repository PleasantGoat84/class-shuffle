<template>
  <div class="home d-flex align-center justify-center fill-height">
    <div>
      <RandomBasket />

      <v-btn color="primary" class="mt-10">抽籤</v-btn>
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
