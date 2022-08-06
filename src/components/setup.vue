<template>
  <el-card class="box-card">
    <h3>Configure Game</h3>
    <el-button @click="emitGameStart">Start Game</el-button>
  </el-card>
</template>

<script>
export default {
  name: 'chess-setup',
  data() {
    return {
      config: {
        whiteTime: 0,
        blackTime: 0,
      },
    };
  },
  methods: {
    emitGameStart() {
      this.$emit('game-start', this.config);
    },
  },
};
</script>

<style scoped>

</style>
