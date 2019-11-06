<template>
  <div :class="`square${data.isWinningSquare ? ' red-background' : ''}`" @click="handleClick">
    <h5>{{ data.val }}</h5>
  </div>
</template>

<script>
export default {
  name: 'Square',
  props: {
      col: {
          type: Number,
          required: true,
      },
      row: {
          type: Number,
          required: true,          
      },
      data: {
          type: Object,
          required: true,
      },
      set: {
          type: Function,
          require: true
      }
  },
  methods: {
    handleClick() {
      this.set(this.row, this.col);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .square {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    padding: 1.8rem;
    font-size: 2rem;
    border: 2px solid black;
    width: 16px;
    height: 16px;
  }
  .square:hover {
    cursor: pointer;
  }
  .red-background {
    background-color: red;
  }
</style>