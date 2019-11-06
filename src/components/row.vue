<template>
  <div class="row">
    <Square v-for="(value, i) in values"
      :key="i"
      :row="idx"
      :col="i"
      :data="value"
      :set="setSquare"
    />
  </div>
</template>

<script>
import Square from './square';
export default {
  name: 'Row',
  components: {
      Square
  },
  props: {
      idx: {
          type: Number,
          required: true,
      },
      values: {
          type: Array,
          required: true,
      },
      setSquare: {
          type: Function,
          required: true,
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .row {
    display: flex;
    flex-direction: row;
    align-items: center;
  }
</style>