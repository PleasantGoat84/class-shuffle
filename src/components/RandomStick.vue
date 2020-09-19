<template>
  <div
    class="stick d-flex align-center"
    :class="{ picked: option.picked }"
    :style="style"
    :id="`stick-${option.id}`"
  >
    <span class="id font-weight-bold" :style="idStyle">{{ option.id }}</span>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import RandomBasket from "@/components/RandomBasket.vue";

export interface StickOption {
  x: number;
  y: number;
  t: number;
  id: number;
  picked: boolean;
}

@Component
export default class RandomStick extends Vue {
  static readonly widthVW = 15 * 1.6;
  static readonly heightVW = RandomStick.widthVW / 15;
  static readonly borderRatio = 0.2;
  static readonly borderWidthVW =
    RandomStick.heightVW * RandomStick.borderRatio;
  static readonly idFontSizeVW =
    (RandomStick.heightVW - RandomStick.borderWidthVW) * 0.75;

  static readonly defaultStyle = {
    width: RandomStick.widthVW + "vw",
    height: RandomStick.heightVW + "vw",

    "border-width": RandomStick.borderWidthVW + "vw",
    "border-radius": RandomStick.heightVW / 2 + "vw"
  };

  @Prop() private option!: StickOption;

  private get style(): { [key: string]: string } {
    return {
      ...RandomStick.defaultStyle,

      left:
        RandomBasket.innerLeftOffsetVW * 1.2 +
        RandomStick.heightVW * RandomStick.borderRatio +
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

  private get idStyle(): { [key: string]: string } {
    return { "font-size": RandomStick.idFontSizeVW + "vw" };
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

  &.picked {
    display: none !important;
  }

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
