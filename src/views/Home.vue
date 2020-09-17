<template>
  <div class="home d-flex align-center justify-center fill-height">
    <div
      class="d-flex flex-column align-center"
      id="basket-outer-wrapper"
      :class="{ mini: status > 0 }"
    >
      <RandomBasket max-count="36" ref="basket" />

      <v-btn
        color="primary"
        class="mt-10"
        id="random-btn"
        v-if="status === 0"
        @click="startRandom()"
      >
        抽籤
      </v-btn>
    </div>

    <div
      class="d-flex flex-column align-start flex-wrap"
      id="name-list"
      v-if="status > 0"
      :class="{ mini: status > 1 }"
    >
      <div v-for="(stu, i) in stuList" :key="i" class="stu">
        <span class="id">
          {{ i + 1 }}
        </span>
        <span class="name">
          {{ stu.name.zh }}
        </span>
        <span
          class="rand-id"
          :class="{ 'not-yet': stuRandId[i] === undefined }"
        >
          {{ stuRandId[i] !== undefined ? stuRandId[i] : "?" }}
        </span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import RandomBasket from "@/components/RandomBasket.vue";

import stuList from "@/data/stu/4/1";

@Component({
  components: {
    RandomBasket
  }
})
export default class Home extends Vue {
  $refs!: {
    basket: RandomBasket;
  };

  private readonly stuList = stuList;

  private status = 0;
  private stuRandId: Array<number> = [];

  private startRandom() {
    this.status = 1;

    setTimeout(() => {
      this.randCallback(0);
    }, 1500);
  }

  private randCallback(i: number) {
    this.$set(this.stuRandId, i, this.$refs.basket.randPick().id);

    if (i < this.stuList.length - 1)
      setTimeout(() => {
        this.randCallback(i + 1);
      }, 100);
    else
      setTimeout(() => {
        this.status = 2;
      }, 3500);
  }
}
</script>

<style lang="scss" scoped>
#basket-outer-wrapper {
  position: relative;

  #random-btn {
    height: 8vh;
    min-width: 10vw;

    position: absolute;
    bottom: -10vh;

    padding: 0 1.25em;
    border-radius: 4vh;

    font-size: 5vh;
  }

  &.mini {
    transform-origin: right bottom;
    transform: scale(0.8, 0.8);

    position: fixed;
    right: 2vw;
    bottom: 2vw;
  }
}

#name-list {
  position: absolute;
  left: 3vw;

  $name-list-height: 95vh;
  $name-per-row: 15;
  max-height: $name-list-height;

  .stu {
    position: relative;

    height: $name-list-height / $name-per-row;
    font-size: $name-list-height / $name-per-row * 0.75;

    text-align: left;
    width: 7.5em;

    padding-left: 1.5em;
    margin-right: 3em;

    .id {
      position: absolute;
      left: 0;
      color: #89bad9;
    }

    .rand-id {
      position: absolute;
      right: 0;

      &.not-yet {
        color: #a3b2bf;
      }
    }
  }
}
</style>
