<template>
  <div class="container">
    <h1 class="title">Doom Fire Effect</h1>
    <a target="_blank" href="https://github.com/cleitonper/doom-fire-algorithm/tree/feature/canvas">
      https://github.com/cleitonper/doom-fire-algorithm/tree/feature/canvas
    </a>
    <fire-wall
      class="firewall"
      ref="firewall"
      :width="width"
      :height="height"
      :debug="debug" />
    <div class="actions">
      <btn-8bit @click.native="decreaseFireIntensity()">-</btn-8bit>
      <btn-8bit @click.native="decreaseFireToMinIntensity()">Min</btn-8bit>
      <btn-8bit @click.native="increaseFireToMaxIntensity()">Max</btn-8bit>
      <btn-8bit @click.native="increaseFireIntensity()">+</btn-8bit>
    </div>
  </div>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';
  import FireWall from '@/components/FireWall.vue';
  import Btn8Bit from '@/components/Btn8Bit.vue';

  @Component({
    components: {
      'fire-wall': FireWall,
      'btn-8bit': Btn8Bit,
    },
  })
  export default class App extends Vue {
    public width = 200;
    public height = 80;

    public $refs!: {
      firewall: FireWall,
    };

    public decreaseFireIntensity() {
      this.$refs.firewall.decreaseIntensity();
    }
    public increaseFireIntensity() {
      this.$refs.firewall.increaseIntensity();
    }

    public decreaseFireToMinIntensity() {
      this.$refs.firewall.decreaseToMinIntensity();
    }

    public increaseFireToMaxIntensity() {
      this.$refs.firewall.increaseToMaxIntensity();
    }
  }
</script>

<style scoped>
  .title {
    font-size: 24px;
    margin: 0px 0px 16px;
  }

  a {
    color: #fff;
    font-size: 10px;
    text-decoration: none;
    margin: 0px 0px 32px;
  }

  .firewall {
    margin-bottom: 18px;
    border: 4px solid  #000;
  }

  .container {
    display: flex;
    flex-flow: column wrap;
    justify-content: flex-start;
    align-items: center;
    background-color: #4444ff;
    box-sizing: border-box;
    padding: 48px 16px;
    min-height: 100vh;
    width: 100%;
  }

  .actions {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    align-items: center;
    width: 220px;
  }
</style>
