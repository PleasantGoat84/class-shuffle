<template>
  <div class="basket-container">
    <RandomStick
      v-for="(option, i) in stickOptions"
      :key="i"
      :option="option"
    />
    <img src="@/assets/basket.png" alt="basket" class="basket" />
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
export default class RandomBasket extends Vue {
  static readonly widthVW = 15;
  static readonly heightVW = RandomBasket.widthVW * 1.09005;
  static readonly innerBottomOffsetVW = RandomBasket.widthVW * 0.14457;
  static readonly innerLeftOffsetVW = (RandomBasket.widthVW * 0.10131) / 2;
  static readonly innerWidthVW =
    RandomBasket.widthVW * 0.89868 -
    RandomStick.heightVH * (window.innerHeight / window.innerWidth);

  private stickOptions: Array<StickOption> = [];

  private genStickOption(): StickOption {
    const x = Math.random();
    const y = Math.random();

    const minRotate = Math.atan(
      RandomBasket.heightVW / ((1 - x) * RandomBasket.innerWidthVW)
    );
    const maxRotate =
      Math.PI -
      Math.atan(RandomBasket.heightVW / (x * RandomBasket.innerWidthVW));

    const t = -(Math.random() * (maxRotate - minRotate) + minRotate);

    return { x, y, t };
  }

  created() {
    for (let i = 0; i < 36; i++) {
      this.stickOptions.push(this.genStickOption());
      // console.debug(this.stickOptions[i]);
      // this.stickOptions.push({ x: 1, t: -Math.PI * 0.5 });
    }
  }
}
</script>

<style lang="scss" scoped>
.basket-container {
  position: relative;

  .basket {
    position: relative;

    margin-top: 30vh;

    width: 15vw;
    z-index: 3;
  }
}
</style>
