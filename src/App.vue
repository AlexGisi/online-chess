<template>
  <el-container>
    <el-header>
      <h1>Chess</h1>
    </el-header>
    <el-main>
      <el-row :gutter="20">
        <el-col :span="12">
          <chess-setup v-if="!gameInProgress" @game-start="startGame"></chess-setup>
          <chess-game-info v-if="gameInProgress"></chess-game-info>
        </el-col>
        <el-col :span="12">
          <chess-board></chess-board>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import ChessBoard from '@/components/board.vue';
import ChessSetup from '@/components/setup.vue';
import ChessGameInfo from '@/components/game-info.vue';

export default {
  name: 'App',
  components: {
    ChessGameInfo,
    ChessSetup,
    ChessBoard,
  },
  data() {
    return {
      gameInProgress: false,
    };
  },
  methods: {
    startGame(config) {
      // eslint-disable-next-line no-console
      console.log(config);
      this.gameInProgress = true;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
