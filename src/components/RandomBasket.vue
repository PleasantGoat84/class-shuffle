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
  private availableStickId: Array<number> = [];

  // generator

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

    return { x, y, t, id: 0, picked: false };
  }

  // methods

  getPicked(idx: number) {
    return this.stickOptions[idx].picked;
  }

  getStickId(idx: number) {
    return this.stickOptions[idx].id;
  }

  pick(idx: number) {
    this.stickOptions[idx].picked = true;
  }

  // hooks

  created() {
    for (let i = 0; i < this.maxCount; i++) {
      this.stickOptions.push(this.genStickOption());
    }

    // random stick ids
    for (let i = 1; i <= this.maxCount; i++) this.availableStickId.push(i);

    for (let i = 0; i < this.maxCount; i++) {
      const idx = Math.floor(Math.random() * this.availableStickId.length);
      this.$set(this.stickOptions[i], "id", this.availableStickId[idx]);
      this.availableStickId.splice(idx, 1);
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
