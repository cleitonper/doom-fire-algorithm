<template>
  <button>
    <slot></slot>
  </button>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';

  @Component
  export default class Btn8Bit extends Vue {}
</script>

<style scoped>
  button {
    margin: 18px 8px 0px;
    padding: 6px 8px 8px 6px;
    font-family: 'Press Start 2P', cursive;
    font-size: 12px;
    cursor: pointer;
    color: #ffffff;
    background-color: #E76E55;
    box-shadow: inset -2px -2px 0px 2px #8C2022;
    display: inline-block;
    border: none;
    position: relative;
  }

  button:focus,
  button:hover {
    box-shadow: inset -3px -3px 0px 3px #8C2022;    background-color: #CE372B;
    background-color: #CE372B;
  }

  button:active {
    box-shadow: inset 3px 3px 0px 3px #8C2022;
  }

  button:focus,
  button:active,
  button:focus:active,
  button::-moz-focus-inner {
    outline: 0;
    outline: none;
    border: 0;
  }

  button::before,
  button::after {
    content: '';
    box-sizing: content-box;
    position: absolute;
    display: block;
    height: 100%;
    width: 100%;
  }

  button::before {
    left: 0;
    top: -4px;
    border-top: 4px solid #000;
    border-bottom: 4px solid #000;
  }

  button::after {
    top: 0;
    left: -4px;
    border-left: 4px solid #000;
    border-right: 4px solid #000;
  }
</style>

