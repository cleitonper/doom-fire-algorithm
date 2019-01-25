<template>
  <td v-if="debug" class="debug" :style="`color: rgb(${getColor(intensity)})`">
    {{ intensity }}
    <span class="index">
      {{ index }}
    </span>
  </td>
  <td v-else :style="`background-color: rgb(${getColor(intensity)})`">

  </td>
</template>

<script lang="ts">
  import { Vue, Component, Prop } from 'vue-property-decorator';
  import { fireColorsPalette } from '@/consts/colors';
  import { RGB } from '@/types/rgb';

  @Component
  export default class FireCell extends Vue {
    @Prop({ default: false })
    public debug!: boolean;

    @Prop()
    public index!: number;

    @Prop()
    public intensity!: number;

    public colors: RGB[] = fireColorsPalette;

    public getColor(intensity: number): string {
      return `${this.colors[intensity].r}, ${this.colors[intensity].g}, ${this.colors[intensity].b}`;
    }
  }
</script>

<style scoped>
  td {
    width: 2px;
    height: 2px;
    padding: 0;
    border: 0;
  }

  td.debug {
    padding: 16px 16px 10px 10px;
    font-size: 10px;
    border: 1px solid #ccc;
    position: relative;
  }

  td.debug .index {
    color: #ccc;
    font-size: 8px;
    position: absolute;
    right: 3px;
    top: 3px;
  }
</style>

