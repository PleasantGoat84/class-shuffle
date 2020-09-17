<template>
  <table class="seat-table" :class="{ zoom: $parent.status > 2 }">
    <tr v-for="(row, i) in cells" :key="i">
      <td v-for="(cell, j) in row" :key="j">
        <span
          class="id"
          v-if="cell.stu === undefined"
          :class="{ 'not-yet': cell.id === undefined }"
        >
          {{ cell.id === undefined ? "?" : cell.id }}
        </span>
        <span class="name" v-else>
          {{ cell.stu.name.zh }}
        </span>
      </td>
    </tr>
  </table>
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
  private randId: { [key: number]: boolean } = {};

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

    setTimeout(handler, 200);
  }

  private randCallback(i: number) {
    let row, col, id;
    do {
      row = Math.floor(Math.random() * this.rowCount);
      col = Math.floor(Math.random() * this.colCount);
      id = Math.floor(Math.random() * this.$parent.stuCount) + 1;
    } while (
      this.cells[row][col].id !== undefined ||
      this.randId[id] !== undefined
    );

    this.randId[id] = true;

    const rowCopy = this.cells[row];
    rowCopy[col] = { id };

    this.$set(this.cells, row, rowCopy);

    if (i > 1)
      setTimeout(() => {
        this.randCallback(i - 1);
      }, 200);
    else
      setTimeout(() => {
        this.matchCallback(0, 0);
      }, 5000);
  }

  created() {
    for (let i = 0; i < this.rowCount; i++) {
      const row = [];
      for (let j = 0; j < this.colCount; j++) row.push({});
      this.cells.push(row);
    }

    setTimeout(() => {
      this.randCallback(this.$parent.stuCount);
    }, 3000);
  }
}
</script>

<style lang="scss" scoped>
.seat-table {
  position: absolute;
  right: 3vw;

  $width: 50vw;
  $height: $width * 0.6;

  width: $width;
  height: $height;

  border-collapse: collapse;

  td {
    width: $width / 6;
    border: solid 0.25em burlywood;

    font-size: ($height / 6 * 0.35);

    .id {
      color: wheat;

      &.not-yet {
        color: #a3b2bf;
      }
    }
  }

  &.zoom {
    position: static;

    width: 75vw;
    height: 75vw * 0.6;

    td {
      width: 75vw / 6;
      font-size: 75vw * 0.6 / 6 * 0.35;
    }
  }
}
</style>
