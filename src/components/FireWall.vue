<template>
  <canvas class="firewall" ref="firewall">
  </canvas>
</template>

<script lang="ts">
  import { Component, Vue, Prop } from 'vue-property-decorator';
  import { fireColorsPalette } from '@/consts/colors';
  import { RGB } from '@/types/rgb';
  const COLORS: RGB[] = fireColorsPalette;
  const PIXEL_MIN_INTENSITY = 0;
  const PIXEL_MAX_INTENSITY = 36;
  const PIXEL_SIZE = 4;

  @Component
  export default class FireWall extends Vue {
    @Prop()
    public width!: number;

    @Prop()
    public height!: number;

    public pixels: number[] = [];

    private baseIntensity = PIXEL_MAX_INTENSITY;

    private context!: CanvasRenderingContext2D;

    public $refs!: {
      firewall: HTMLCanvasElement;
    };

    get area(): number {
      return this.width * this.height;
    }

    private initializeDrawArea(): void {
      this.$refs.firewall.width = this.width * PIXEL_SIZE;
      this.$refs.firewall.height = this.height * PIXEL_SIZE;
      this.context = this.$refs.firewall.getContext('2d') as CanvasRenderingContext2D;
    }

    public initializeFireDataSource(): void {
      this.pixels = new Array(this.area).fill(PIXEL_MIN_INTENSITY);
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

    private renderFire(): void {
      for (let row = 0; row < this.height; ++row) {
        for (let column = 0; column < this.width; ++column) {
          const index = column + (row * this.width);
          const intensity = this.pixels[index];
          const color = COLORS[intensity];
          const colorString = `${color.r}, ${color.g}, ${color.b}`;

          this.context.fillStyle = `rgb(${colorString})`;
          this.context.fillRect(column * PIXEL_SIZE, row * PIXEL_SIZE, PIXEL_SIZE, PIXEL_SIZE);
        }
      }
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

      this.renderFire();
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
    }

    private mounted() {
      this.initializeFirePropagation();
      this.initializeDrawArea();
    }
  }
</script>