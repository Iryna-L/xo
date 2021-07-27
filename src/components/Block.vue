<template>
  <div class="block" :class="{'taken': blockInfo.taken,
  'winner': blockInfo.winner }" @click="go">
    <span> {{ blockInfo.value }} </span>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        
      }
    },
    props: {
      blockInfo: {
        type: Object,
        required: true
      },
    },
    methods: {
      go() {
        if (this.blockInfo.taken) return
        console.log('go')
        this.$emit('go')
      }
    }
  }
</script>

<style>
.block {
  border: 1px solid black;
  margin: 5px;
  width: 50px;
  height: 50px;
  cursor: pointer;
}
.taken {
  background-color: rgba(0,0,0,.4);
  cursor: not-allowed;
}
.winner {
  background-color: #90ee90;
}
</style>