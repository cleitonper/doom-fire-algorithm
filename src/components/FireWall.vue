<template>
  <table>
    <fire-row
      v-for="row in height"
      :key="row">
      <fire-cell
        :debug="debug"
        v-for="column in width"
        :key="calculatePixelIndex(row, column, width)"
        :index="calculatePixelIndex(row, column, width)"
        :intensity="pixels[calculatePixelIndex(row, column, width)]">
      </fire-cell>
    </fire-row>
  </table>
</template>

<script lang="ts">
  import { Component, Vue, Prop } from 'vue-property-decorator';
  import FireRow from '@/components/FireRow.vue';
  import FireCell from '@/components/FireCell.vue';
  import { fireColorsPalette } from '@/consts/colors';
  import { RGB } from '@/types/rgb';
  const PIXEL_MIN_INTENSITY = 0;
  const PIXEL_MAX_INTENSITY = 36;

  @Component({
    components: {
      'fire-row': FireRow,
      'fire-cell': FireCell,
    },
  })
  export default class FireWall extends Vue {
    @Prop()
    public debug!: boolean;

    @Prop()
    public width!: number;

    @Prop()
    public height!: number;

    public pixels: number[] = [];

    private colors: RGB[] = fireColorsPalette;

    private baseIntensity = PIXEL_MAX_INTENSITY;

    get area(): number {
      return this.width * this.height;
    }

    public initializeFireDataSource() {
      this.pixels = new Array(this.area).fill(PIXEL_MIN_INTENSITY);
    }

    private getColor(intensity: number): string {
      return `${this.colors[intensity].r}, ${this.colors[intensity].g}, ${this.colors[intensity].b}`;
    }

    private calculatePixelIndex(row: number, column: number, width: number): number {
      return (column + ((row * width) - width)) - 1;
    }

    private generateFireSource(intensity: number = PIXEL_MAX_INTENSITY): void {
      const FIRE_FIRST_COLUMN = 0;
      const FIRE_LAST_COLUMN = this.width - 1;
      this.baseIntensity = intensity;
      this.pixels = this.pixels
      .reverse()
      .map(
        (currentIntensity, index) => (index >= FIRE_FIRST_COLUMN && index <= FIRE_LAST_COLUMN)
          ? this.baseIntensity
          : currentIntensity,
      ).
      reverse();
    }

    private updateFirePixelIntensity(index: number): number {
      const belowPixelIndex = index + this.width;

      if (belowPixelIndex >= this.area) {
        return -1;
      }

      const decay = Math.floor(Math.random() * 3);
      const belowPixenlIntensity = this.pixels[belowPixelIndex - decay];
      const newPixelIndex = (belowPixenlIntensity - decay >= 0)
        ? belowPixenlIntensity - decay
        : 0;

      return newPixelIndex;
    }

    private calculateFirePropagation(): void {
      const FIRE_FIRST_COLUMN = 0;
      const FIRE_LAST_COLUMN = this.width - 1;
      this.pixels = this.pixels.map(
        (currentPixelIntensity, index) => this.updateFirePixelIntensity(index) >= 0
          ? this.updateFirePixelIntensity(index)
          : currentPixelIntensity,
      );
    }

    private initializeFirePropagation(): number {
      return setInterval(() => this.calculateFirePropagation(), 50);
    }

    public decreaseIntensity() {
      const decreaseAmount = 5;
      const currentIntensity = this.baseIntensity;
      const newIntensity = (currentIntensity - decreaseAmount < PIXEL_MIN_INTENSITY)
        ? PIXEL_MIN_INTENSITY
        : currentIntensity - decreaseAmount;
      this.generateFireSource(newIntensity);
    }

    public increaseIntensity() {
      const increaseAmount = 5;
      const currentIntensity = this.baseIntensity;
      const newIntensity = (currentIntensity + increaseAmount > PIXEL_MAX_INTENSITY)
        ? PIXEL_MAX_INTENSITY
        : currentIntensity + increaseAmount;
      this.generateFireSource(newIntensity);
    }

    public decreaseToMinIntensity() {
      this.generateFireSource(PIXEL_MIN_INTENSITY);
    }

    public increaseToMaxIntensity() {
      this.generateFireSource(PIXEL_MAX_INTENSITY);
    }

    private created() {
      this.initializeFireDataSource();
      this.generateFireSource();
      this.initializeFirePropagation();
    }
  }
</script>

<style>
  table {
    border-collapse: collapse;
    border: 6px solid #000;
  }

  td {
    padding: 12px;
    border: 1px solid #ccc;
    position: relative;
  }

  td .index {
    color: #ccc;
    font-size: 10px;
    position: absolute;
    right: 3px;
    top: 3px;
  }
</style>
