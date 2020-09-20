<template>
  <div class="seat-table d-flex" :class="{ zoom: $parent.status > 2 }">
    <div
      v-for="(row, i) in cells"
      :key="i"
      class="seat-col d-flex flex-column align-stretch flex-shrink-0"
    >
      <div
        v-for="(cell, j) in row"
        :key="j"
        class="seat flex-shrink-0 d-inline-flex flex-column justify-center"
      >
        <span
          class="id"
          v-if="cell.stu === undefined"
          :class="{ 'not-yet': cell.id === undefined }"
        >
          {{ cell.id === undefined ? "?" : cell.id }}
        </span>
        <span
          class="name d-inline-flex flex-column justify-center"
          v-else
          :class="{ red: cell.stu.name.zh === '張鎮揚' && false }"
        >
          <span class="zh">
            {{ cell.stu.name.zh }}
          </span>
          <span class="en">
            {{ cell.stu.name.en }}
          </span>
        </span>
      </div>
    </div>
    <template v-if="$parent.status > 2">
      <span class="teacher-table">教壇</span>
      <span class="door">門口</span>
    </template>
  </div>
</template>

<script lang="ts">
import Home from "@/views/Home.vue";
import { Component, Vue } from "vue-property-decorator";

interface Cell {
  id?: number;
  stu?: {
    name: {
      zh: string;
    };
  };
}

@Component
export default class SeatTable extends Vue {
  $parent!: Home;

  private readonly rowCount = 6;
  private readonly colCount = 6;

  private cells: Array<Array<Cell>> = [];
  private availableSeatId: Array<number> = [];

  private matchCallback(r: number, c: number) {
    // debugger;
    const randId = this.$parent.getStuRandId();
    const stu = this.$parent.getStu(
      randId.findIndex(id => id === this.cells[r][c].id)
    );

    const rowCopy = this.cells[r];
    rowCopy[c] = { stu };

    this.$set(this.cells, r, rowCopy);

    let handler: () => void;

    if (c + 1 <= this.colCount - 1)
      handler = () => {
        this.matchCallback(r, c + 1);
      };
    else if (c + 1 > this.colCount - 1 && r + 1 <= this.rowCount - 1)
      handler = () => {
        this.matchCallback(r + 1, 0);
      };
    else
      handler = () => {
        this.$parent.status = 3;
      };

    setTimeout(handler, 100);
  }

  private randCallback(i: number) {
    const row = Math.floor(i / this.rowCount);
    const col = i % this.colCount;

    const idx = Math.floor(Math.random() * this.availableSeatId.length);

    const rowCopy = this.cells[row];
    rowCopy[col] = { id: this.availableSeatId[idx] };

    this.$set(this.cells, row, rowCopy);

    this.availableSeatId.splice(idx, 1);

    if (i < this.rowCount * this.colCount - 1)
      setTimeout(() => {
        this.randCallback(i + 1);
      }, 100);
    else
      setTimeout(() => {
        this.matchCallback(0, 0);
      }, 2500);
  }

  created() {
    for (let i = 0; i < this.rowCount; i++) {
      const row = [];
      for (let j = 0; j < this.colCount; j++) row.push({});
      this.cells.push(row);
    }

    // random seat ids
    for (let i = 1; i <= this.rowCount * this.colCount; i++)
      this.availableSeatId.push(i);

    setTimeout(() => {
      this.randCallback(0);
    }, 1500);
  }
}
</script>

<style lang="scss" scoped>
.seat-table {
  position: absolute;
  right: 3vw;

  .seat {
    $height: 7vw;

    height: $height;
    width: 10vw;

    border: solid 0.25em burlywood;
    margin: 0.15vh 1vw;

    font-size: $height / 4;

    .id {
      color: wheat;

      &.not-yet {
        color: #a3b2bf;
      }
    }

    .name {
      .en {
        font-size: 70%;
      }
    }
  }

  &.zoom {
    position: static;

    transform: scale(1.1);
  }
}

.teacher-table,
.door {
  position: fixed;
  bottom: 0;

  font-size: 3vw;

  &.teacher-table {
    left: -2.5em;
  }

  &.door {
    right: -2.5em;
  }
}
</style>
