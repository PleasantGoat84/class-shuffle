<template>
  <div class="basket-container" ref="container">
    <RandomStick
      v-for="(option, i) in stickOptions"
      :key="i"
      :option="option"
    />
    <img src="@/assets/basket.png" alt="basket" class="basket" />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import RandomStick, { StickOption } from "@/components/RandomStick.vue"; // @ is an alias to /src

@Component({
  components: {
    RandomStick
  }
})
export default class RandomBasket extends Vue {
  // props
  @Prop() private maxCount!: number;

  // statics

  static readonly widthVW = 15;
  static readonly heightVW = RandomBasket.widthVW * 1.09005;
  static readonly innerBottomOffsetVW = RandomBasket.widthVW * 0.14457;
  static readonly innerLeftOffsetVW = (RandomBasket.widthVW * 0.10131) / 2;
  static readonly innerWidthVW =
    RandomBasket.widthVW * 0.89868 -
    RandomStick.heightVH * (window.innerHeight / window.innerWidth);

  // stick stuff

  private stickOptions: Array<StickOption> = [];
  private stickId: { [key: number]: boolean } = {};

  // generator

  private genStickOption(): StickOption {
    let id;
    do {
      id = Math.floor(Math.random() * this.maxCount);
      // console.debug(id);
    } while (this.stickId[id] !== undefined);
    this.stickId[id] = true;

    const x = Math.random();
    const y = Math.random();

    const minRotate = Math.atan(
      RandomBasket.heightVW / ((1 - x) * RandomBasket.innerWidthVW)
    );
    const maxRotate =
      Math.PI -
      Math.atan(RandomBasket.heightVW / (x * RandomBasket.innerWidthVW));

    const t = -(Math.random() * (maxRotate - minRotate) + minRotate);

    return { x, y, t, id: id + 1, picked: false };
  }

  // methods

  randPick(): StickOption {
    let idx;

    do {
      idx = Math.floor(Math.random() * this.stickOptions.length);
    } while (this.stickOptions[idx].picked);

    this.stickOptions[idx].picked = true;

    return this.stickOptions[idx];
  }

  // hooks

  created() {
    for (let i = 0; i < this.maxCount; i++) {
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
  width: 15vw;

  .basket {
    position: relative;

    margin-top: 18vh;

    width: 15vw;
    z-index: 3;
  }
}
</style>
