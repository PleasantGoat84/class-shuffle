<template>
  <div
    class="stick d-flex align-center"
    :style="style"
    :id="`stick-${option.id}`"
  >
    <span class="id font-weight-bold" :style="idStyle">{{ option.id }}</span>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import RandomBasket from "./RandomBasket.vue";

export interface StickOption {
  x: number;
  y: number;
  t: number;
  id: number;
}

@Component
export default class RandomStick extends Vue {
  static readonly widthVH = 50;
  static readonly heightVH = RandomStick.widthVH / 13;
  static readonly borderRatio = 0.2;
  static readonly borderWidthVH =
    RandomStick.heightVH * RandomStick.borderRatio;
  static readonly idFontSizeVH =
    (RandomStick.heightVH - RandomStick.borderWidthVH) * 0.75;

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
        RandomBasket.innerBottomOffsetVW + (this.option.y * 4 - 1.5) + "vw",

      transform: `rotateZ(${this.option.t}rad)`,

      background: `linear-gradient(${
        this.option.t < -Math.PI / 2 ? "to top" : "to bottom"
      },  wheat, #f9d694)`
    };
  }

  get idStyle(): { [key: string]: string } {
    return { "font-size": RandomStick.idFontSizeVH + "vh" };
  }
}
</script>

<style lang="scss" scoped>
.stick {
  // background: wheat;
  border: solid burlywood;

  z-index: 2;

  // most positioning handled in :style
  transform-origin: center left;

  position: absolute;

  // text display
  .id {
    position: relative;
    left: 0.75em;

    color: peru;
    // padding-left: 0.75em;
    transform-origin: center center;
    transform: rotateZ(-90deg);
  }
}
</style>
