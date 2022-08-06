<template>
  <div id="myBoard" style="width: 400px"></div>

  <p>{{ gameStatus }}</p>
  <p>{{ fen }}</p>
  <p>{{ pgn }}</p>
</template>

<script>
import ChessBoard from 'chessboardjs-vue';
import { Chess } from 'chess.js';

export default {
  name: 'chess-board',
  data() {
    return {
      board: null,
      game: Chess(),
      gameStatus: 'status',
      fen: 'fen',
      pgn: 'pgn',
    };
  },

  mounted() {
    const config = {
      draggable: true,
      position: 'start',
      onDragStart: this.onDragStart,
      onDrop: this.onDrop,
      onSnapEnd: this.onSnapEnd,
    };
    this.board = ChessBoard('myBoard', config);

    this.updateStatus();
  },

  methods: {
    // eslint-disable-next-line consistent-return,no-unused-vars
    onDragStart(source, piece, position, orientation) {
      // do not pick up pieces if the game is over
      if (this.game.game_over()) return false;

      // only pick up pieces for the side to move
      if ((this.game.turn() === 'w' && piece.search(/^b/) !== -1)
        || (this.game.turn() === 'b' && piece.search(/^w/) !== -1)) {
        return false;
      }
    },
    // eslint-disable-next-line consistent-return
    onDrop(source, target) {
      const move = this.game.move({
        from: source,
        to: target,
        promotion: 'q', // TODO
      });

      if (move === null) return 'snapback';

      this.updateStatus();
    },
    updateStatus() {
      let tempStatus = '';

      let moveColor = 'White';
      if (this.game.turn() === 'b') {
        moveColor = 'Black';
      }

      if (this.game.in_checkmate()) {
        tempStatus = `Game over, ${moveColor} is in checkmate.`;
      } else if (this.game.in_draw()) {
        tempStatus = 'Game over, drawn position';
      } else {
        tempStatus = `${moveColor} to move`;

        if (this.game.in_check()) {
          tempStatus += `, ${moveColor} is in check`;
        }
      }

      this.gameStatus = tempStatus;
      this.fen = this.game.fen();
      this.pgn = this.game.pgn();
    },
    onSnapEnd() {
      this.board.position(this.game.fen());
    },
  },
};
</script>

<style scoped>
#myBoard {
  margin: auto;
}
</style>
