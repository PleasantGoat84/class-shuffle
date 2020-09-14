<template>
  <div
    class="stick"
    :style="{
      transform: `rotateZ(${option.k}rad)`,
      left: `${6 + 5.5 * option.x}vw`
    }"
  />
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

export interface StickOption {
  x: number;
  k: number;
}

@Component
export default class RandomStick extends Vue {
  @Prop() private option!: StickOption;
}
</script>

<style lang="scss" scoped>
.stick {
  $stick-width: 50vh;
  $stick-height: $stick-width / 13;

  width: $stick-width;
  height: $stick-height;

  background: wheat;
  border: $stick-height / 5 solid burlywood;

  border-radius: $stick-height / 2;

  // most positioning handled in :style
  transform-origin: center left;

  position: absolute;
  bottom: 5vh;
}
</style>
