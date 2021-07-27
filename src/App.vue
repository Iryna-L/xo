<template>
  <div id="app">
    <div>Chose X or 0</div>
    <div>
      <input type="radio" id="X" name="age" value="X" v-model="play" />
      <label for="X">X</label>
    </div>
    <div>
      <input type="radio" id="O" name="age" value="O" v-model="play" />
      <label for="O">O</label>
    </div>

    <div ref="game" class="game">
      <div v-for="(block) in blocks" :key="block.key">
        <Block @go="handleGo(block.key)" :block-info="block" />
      </div>
    </div>
    <div v-if="winner">
      {{ winner }} won
    </div>
  </div>
</template>

<script>
import Block from "./components/Block.vue";

export default {
  name: "App",
  data() {
    return {
      play: "x",
      playBot: "o",
      blocks: [
        {
          value: '',
          key: 0,
          taken: false,
          winner: false,
          row: 'r1',
          col: 'c1',
          diag: 'd1'
        },
        {
          value: '',
          key: 1,
          taken: false,
          winner: false,
          row: 'r1',
          col: 'c2'
        },
        {
          value: '',
          key: 2,
          taken: false,
          winner: false,
          row: 'r1',
          col: 'c3',
          diag: 'd2'
        },
        {
          value: '',
          key: 3,
          taken: false,
          winner: false,
          row: 'r2',
          col: 'c1'
        },
        {
          value: '',
          key: 4,
          taken: false,
          winner: false,
          row: 'r2',
          col: 'c2',
          diag: ['d1', 'd2']
        },
        {
          value: '',
          key: 5,
          taken: false,
          winner: false,
          row: 'r2',
          col: 'c3'
        },
        {
          value: '',
          key: 6,
          taken: false,
          winner: false,
          row: 'r3',
          col: 'c1',
          diag: 'd2'
        },
        {
          value: '',
          key: 7,
          taken: false,
          winner: false,
          row: 'r3',
          col: 'c2'
        },
        {
          value: '',
          key: 8,
          taken: false,
          winner: false,
          row: 'r3',
          col: 'c3',
          diag: 'd1'
        },
      ],
      filled: {
        x: [],
        o: []
      },
      winner: false
    };
  },
  mounted() {
    
  },
  computed: {
    emptyBlocks() {
      return this.blocks.filter(block => {
        return !block.taken
      })
    },
   
  },
  components: {
    Block,
  },
  methods: {
    checkWinner(player) {
      const length = this.filled[player].length
      console.log(this.filled[player].flat())
      //const arrFlatted = this.filled[player].flat()
      const unique = new Set(this.filled[player].flat()).size

      console.log('u',unique)

      let diff = length - unique
      if (diff === 3) {
        this.winner = player
      }
    },
    handleGo(index) {
      this.blocks[index].value = this.play;
      this.blocks[index].taken = true;

      this.filled[this.play].push(this.blocks[index].row)
      this.filled[this.play].push(this.blocks[index].col)
      if (this.blocks[index].diag) {
        this.filled[this.play].push(this.blocks[index].diag)
      }
      this.checkWinner(this.play)
      
      

      setTimeout(() => {
        this.goBot()
      }, 1000)
      
    },
    goBot() {
      let number = Math.floor(Math.random() * this.emptyBlocks.length);
      console.log('numb', number)
      let available = this.emptyBlocks[number].key
      console.log('av', this.emptyBlocks[number])
      this.checkWinner(this.playBot)

     
      this.blocks[available].value = this.playBot;
      this.blocks[available].taken = true;
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
  width: 160px;
  height: 160px;
  margin: 0 auto;
}
</style>
