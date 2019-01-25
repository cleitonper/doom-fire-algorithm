<template>
  <div class="container">
    <h1 class="title">Doom Fire Effect</h1>
    <a target="_blank" href="https://github.com/cleitonper/doom-fire-algorithm">
      https://github.com/cleitonper/doom-fire-algorithm
    </a>
    <fire-wall
      class="firewall"
      ref="firewall"
      :width="width"
      :height="height"
      :debug="debug" />
    <div class="actions">
      <btn-8bit @click.native="toggleDebug()">Toggle Debug</btn-8bit>
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
    public debug = false;
    public width = 20;
    public height = 13;

    public $refs!: {
      firewall: FireWall,
    };

    public toggleDebug(): void {
      this.debug = !this.debug;
    }

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

  .actions button:first-child {
    display: block;
    width: 100%;
  }
</style>
