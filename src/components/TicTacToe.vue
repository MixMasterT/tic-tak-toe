<template>
  <div class="tic-tac-toe">
    <div class="turn-description"><h3>{{ message }}</h3></div>
    <div class="board">
      <Row v-for="(row, i) in game"
        :key="i"
        :idx="i"
        :values="row"
        :setSquare="setSquareValue"
      />
    </div>
    <div class="game-controls">
      <button @click="undoMove()">undo</button>
      <button @click="resetGame()">reset</button>
    </div>
  </div>
</template>

<script>
import Row from './row';
const newGame = [ // [row][col]
    [{}, {}, {}],
    [{}, {}, {}],
    [{}, {}, {}],
];
export default {
  name: 'TicTacToe',
  components: {
    Row
  },
  computed: {
    message() {
      return this.winner ? `${this.winner} is the winner!` : `It's ${this.currentPlayer}'s turn.`;
    }
  },
  data() {
    return {
      game: this.deepCopyGame(newGame),
      currentPlayer: Math.random() > 0.5 ? 'x' : 'o',
      history: [],
      winner: null,
    }
  },
  methods: {
    setSquareValue(row, col) {
      const game = this.game;
      this.history.push(this.deepCopyGame(game));
      if(game[row][col].val) return;
      // new row is old row but with a new .val property for the data in the target square
      const newRow = game[row].map((data, i) => col === i ? Object.assign({}, data, { val: this.currentPlayer }) : data);
      game.splice(row, 1, newRow); // Must use splice here... setting Object/Array properties by index is not reactive
      this.checkForWin();
      this.swapCurrentPlayer();
    },
    swapCurrentPlayer() {
      this.currentPlayer = 'x' === this.currentPlayer ? 'o' : 'x';
    },
    checkForWin() {
      const g = this.game;
      // lines is in a specific order rows (3), cols (3), diags (2)
      let lines = g.slice()  // rows
        .concat(g.map((row, i) => g.map(col => col[i]))) // columns
        .concat([ // diagonals
          [g[0][0], g[1][1], g[2][2]],
          [g[0][2], g[1][1], g[2][0]]
        ]);
      const winningLineIdx = lines.findIndex(l => l.every(sqData => l[0].val && sqData.val === l[0].val))
      if(winningLineIdx > -1) {
        const winningSquares = [];
        switch(winningLineIdx) {
          case 0:
            winningSquares.push([0,0], [0, 1], [0, 2]);
            break;
          case 1:
            winningSquares.push([1,0], [1, 1], [1, 2]);
            break;
          case 2:
            winningSquares.push([2,0], [2, 1], [2, 2]);
            break;
          case 3:
            winningSquares.push([0, 0], [1, 0], [2, 0]);
            break;
          case 4:
            winningSquares.push([0, 1], [1, 1], [2, 1]);
            break;
          case 5:
            winningSquares.push([0, 2], [1, 2], [2, 2]);
            break;
          case 6:
            winningSquares.push([0, 0], [1, 1], [2, 2]);
            break;
          case 7:
            winningSquares.push([0, 2], [1, 1], [2, 0]);
            break;
        }
        const gameWithWinMarks = this.deepCopyGame(this.game);
        winningSquares.forEach(rowColPair => {
          const row = rowColPair[0];
          const col = rowColPair[1];
          gameWithWinMarks[row][col].isWinningSquare = true;
        });
        this.winner = this.currentPlayer;
        this.game = gameWithWinMarks;
        return true;
      }
      return false;
    },
    resetGame() {
      this.game = this.deepCopyGame(newGame);
      this.history = [];
      this.winner = null;
    },
    deepCopyGame(game) {
      return game.map(row => row.map(sqData => Object.assign({}, sqData)));
    },
    undoMove() {
      if(this.history.length < 1) return;
      this.game = this.history[this.history.length - 1];
      this.history = this.history.slice(0, this.history.length - 1);
      this.swapCurrentPlayer();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .tic-tac-toe {
    display: flex;
    flex-direction: column;
    align-items: center;

  }
  .board {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
