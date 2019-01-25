<template>
  <tr class="table-row">
    <slot></slot>
  </tr>
</template>

<script lang="ts">
  import { Vue, Component } from 'vue-property-decorator';

  export default class FireRow extends Vue {}
</script>
