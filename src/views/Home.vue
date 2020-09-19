<template>
  <div class="home d-flex align-center justify-center" style="height: 100%;">
    <div
      class="d-flex flex-column align-center"
      id="basket-outer-wrapper"
      :class="{ mini: status > 0 }"
      v-if="status < 2"
    >
      <RandomBasket :max-count="stuCount" ref="basket" />

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
      v-if="status > 0 && status < 3"
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

    <SeatTable v-if="status > 1" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import RandomBasket from "@/components/RandomBasket.vue";
import SeatTable from "@/components/SeatTable.vue";

import stuList from "@/data/stu/4/1";

@Component({
  components: {
    RandomBasket,
    SeatTable
  }
})
export default class Home extends Vue {
  $refs!: {
    basket: RandomBasket;
  };

  private readonly stuList = stuList;

  readonly stuCount = 36;

  status = 0;
  private stuRandId: Array<number> = [];

  private availableStuSticks: Array<number> = [];

  getStuRandId() {
    return this.stuRandId;
  }

  getStu(id: number) {
    return this.stuList[id];
  }

  private startRandom() {
    this.status = 1;

    setTimeout(() => {
      this.randCallback(0);
    }, 750);
  }

  private randCallback(i: number) {
    const idx = Math.floor(Math.random() * this.availableStuSticks.length);

    this.$refs.basket.pick(idx);

    this.$set(
      this.stuRandId,
      i,
      this.$refs.basket.getStickId(this.availableStuSticks[idx])
    );

    this.availableStuSticks.splice(idx, 1);

    if (i < this.stuList.length - 1)
      setTimeout(() => {
        this.randCallback(i + 1);
      }, 100);
    else
      setTimeout(() => {
        this.status = 2;
      }, 2500);
  }

  created() {
    // random stu sticks
    for (let i = 0; i < this.stuCount; i++) this.availableStuSticks.push(i);
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
    // transform: scale(0.8, 0.8);

    position: fixed;
    right: 2vw;
    bottom: 2vw;
  }
}

#name-list {
  position: absolute;
  left: 3vw;

  $name-list-height: 95vh;
  $name-per-row: 20;
  max-height: $name-list-height;

  $stu-width: 7em;

  .stu {
    position: relative;

    height: $name-list-height / $name-per-row;
    font-size: $name-list-height / $name-per-row * 0.75;

    text-align: left;
    width: $stu-width;

    padding-left: 1.5em;
    margin-right: 2em;

    .id {
      position: absolute;
      left: 0;
      color: #89bad9;
    }

    .rand-id {
      position: absolute;
      right: 0;

      color: burlywood;

      &.not-yet {
        color: #a3b2bf;
      }
    }
  }

  &.mini {
    .stu {
      width: $stu-width - 4.5em;

      .name {
        display: none;
      }
    }
  }
}
</style>
