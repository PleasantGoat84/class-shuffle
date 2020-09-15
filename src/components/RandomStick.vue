<template>
  <div class="stick" :style="style" />
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import RandomBasket from "./RandomBasket.vue";

export interface StickOption {
  x: number;
  y: number;
  t: number;
}

@Component
export default class RandomStick extends Vue {
  static readonly widthVH = 50;
  static readonly heightVH = RandomStick.widthVH / 13;
  static readonly borderRatio = 0.2;
  static readonly borderWidthVH =
    RandomStick.heightVH * RandomStick.borderRatio;

  static readonly defaultStyle = {
    width: RandomStick.widthVH + "vh",
    height: RandomStick.heightVH + "vh",

    "border-width": RandomStick.borderWidthVH + "vh",
    "border-radius": RandomStick.heightVH / 2 + "vh"
  };

  @Prop() private option!: StickOption;

  get style(): { [key: string]: string } {
    return {
      ...RandomStick.defaultStyle,

      left:
        RandomBasket.innerLeftOffsetVW +
        RandomStick.heightVH * RandomStick.borderRatio +
        this.option.x * RandomBasket.innerWidthVW +
        "vw",

      bottom:
        RandomBasket.innerBottomOffsetVW + (this.option.y * 1 - 0.5) + "vw",

      transform: `rotateZ(${this.option.t}rad)`
    };
  }
}
</script>

<style lang="scss" scoped>
.stick {
  background: wheat;
  border: solid burlywood;

  z-index: 2;

  // most positioning handled in :style
  transform-origin: center left;

  position: absolute;
}
</style>
