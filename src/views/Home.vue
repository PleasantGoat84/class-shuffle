<template>
  <div class="home d-flex align-center justify-center fill-height">
    <div>
      <div class="basket-container">
        <RandomStick
          v-for="(option, i) in stickOptions"
          :key="i"
          :option="option"
        />
        <v-img src="@/assets/basket.png" width="15vw" class="mx-auto basket" />
      </div>

      <v-btn color="primary" class="mt-10">抽籤</v-btn>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import RandomStick, { StickOption } from "@/components/RandomStick.vue"; // @ is an alias to /src

@Component({
  components: {
    RandomStick
  }
})
export default class Home extends Vue {
  readonly basketHeight = window.innerWidth * 0.13;
  readonly basketWidth = window.innerWidth * 0.1;

  private stickOptions: Array<StickOption> = [];

  created() {
    for (let i = 0; i < 18; i++) {
      const x = Math.random();
      const exactX = x * this.basketWidth;

      const minRotate = Math.atan(
        this.basketHeight / (this.basketWidth - exactX)
      );
      const maxRotate = Math.PI - Math.atan(this.basketHeight / exactX);
      const k = -(Math.random() * (maxRotate - minRotate) + minRotate);

      console.debug(x, minRotate, maxRotate, k);

      this.stickOptions.push({ x, k });
    }
  }
}
</script>

<style lang="scss" scoped>
.basket-container {
  position: relative;

  .basket {
    margin-top: 30vh;
  }
}
</style>
