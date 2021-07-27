<template>
  <div id="app">
    <div>Chose X or 0</div>
    <div>
      <input type="radio" id="X" name="player" value="x" v-model="player" />
      <label for="X">X</label>
    </div>
    <div>
      <input type="radio" id="O" name="player" value="o" v-model="player" />
      <label for="O">O</label>
    </div>

    <div class="game" :class="{'game-ended': gameEnded}">
      <div v-for="block in blocks" :key="block.key">
        <div
          class="block"
          :class="{ taken: block.taken, winner: block.winner }"
          @click="handleGo(block.key)"
        >
          <span> {{ block.value }} </span>
        </div>
      </div>
    </div>
    <div v-if="gameEnded">
      <div v-if="winner">
        {{ winner }} won
      </div>
      <div v-if="gameEnded && !winner">
        Draw
      </div>
      <button @click="clearGame">Play again</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      player: "x",
      blocks: [
        {
          value: "",
          key: 0,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 1,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 2,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 3,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 4,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 5,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 6,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 7,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 8,
          taken: false,
          winner: false,
        },
      ],
      filled: {
        x: [],
        o: [],
      },
      winner: false,
    };
  },
  mounted() {},
  computed: {
    emptyBlocks() {
      return this.blocks.filter((block) => {
        return !block.taken;
      });
    },
    playerBot() {
      return this.player === "x" ? "o" : "x";
    },
    gameEnded() {
      return this.winner || !this.emptyBlocks.length
    }
  },
  watch: {
    player(val) {
      this.clearGame()
      if (val === 'o') {
        this.goBot()
      }
    }
  },
  methods: {
    markWinner(player, arrKeys) {
      this.winner = player;
      this.blocks = this.blocks.map((b) => {
        if (arrKeys.includes(b.key)) {
          b.winner = true;
        }
        return b;
      });
    },
    checkWinner(player) {
      const arr = this.filled[player];

      if (arr.includes(0) && arr.includes(1) && arr.includes(2)) {
        this.markWinner(player, [0, 1, 2]);
      } else if (arr.includes(3) && arr.includes(4) && arr.includes(5)) {
        this.markWinner(player, [3, 4, 5]);
      } else if (arr.includes(6) && arr.includes(7) && arr.includes(8)) {
        this.markWinner(player, [6, 7, 8]);
      } else if (arr.includes(0) && arr.includes(3) && arr.includes(6)) {
        this.markWinner(player, [0, 3, 6]);
      } else if (arr.includes(1) && arr.includes(4) && arr.includes(7)) {
        this.markWinner(player, [1, 4, 7]);
      } else if (arr.includes(2) && arr.includes(5) && arr.includes(8)) {
        this.markWinner(player, [2, 5, 8]);
      } else if (arr.includes(0) && arr.includes(4) && arr.includes(8)) {
        this.markWinner(player, [0, 4, 8]);
      } else if (arr.includes(2) && arr.includes(4) && arr.includes(6)) {
        this.markWinner(player, [2, 4, 6]);
      }
    },
    handleGo(index) {
      if (this.blocks[index].taken) return;
      this.blocks[index].value = this.player;
      this.blocks[index].taken = true;

      this.filled[this.player].push(this.blocks[index].key);
      this.checkWinner(this.player);

      if (this.winner) return;

      this.goBot();
    },
    goBot() {
      if (!this.emptyBlocks.length) return
      let number = Math.floor(Math.random() * this.emptyBlocks.length);
      let available = this.emptyBlocks[number].key;
      this.filled[this.playerBot].push(this.blocks[available].key);
      this.checkWinner(this.playerBot);
      this.blocks[available].value = this.playerBot;
      this.blocks[available].taken = true;
    },
    clearGame() {
      this.blocks = [
        {
          value: "",
          key: 0,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 1,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 2,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 3,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 4,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 5,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 6,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 7,
          taken: false,
          winner: false,
        },
        {
          value: "",
          key: 8,
          taken: false,
          winner: false,
        },
      ]
      this.filled = {
        x: [],
        o: [],
      }
      this.winner = false
    }
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
.game {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  width: 185px;
  height: 185px;
  margin: 0 auto;
}
.game-ended {
  position: relative;
  z-index: -1;
}
.block {
  border: 1px solid black;
  margin: 5px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  align-items: center;
  display: flex;
  justify-content: center;
  font-size: 30px;
}
.taken {
  background-color: rgba(0, 0, 0, 0.4);
  cursor: not-allowed;
}
.winner {
  background-color: #90ee90;
}
</style>
