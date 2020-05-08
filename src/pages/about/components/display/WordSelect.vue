<template>
  <div class="container" @touchstart="start" @touchmove="move" @touchend="end">
    <Word v-for="(word, index) in corpus" :key="index+word.content" :index="index" :word="word" @recoverWord="recoverWord" @getCharPosition="getCharPosition" />
  </div>
</template>

<script>
import Word from './components/Word/Word'
export default {
  components:{
      Word
  },
  props:{
      corpus:{
          type:Array
      }
  },
  data () {
    return {
      charPosition: {},
      touchStartIndex: -1,
      touchEndIndex:-1
    }
  },

  watch: {
    corpus () {
      this.$forceUpdate()
    }
  },

  methods:{
    getCharPosition (index, position) {
      this.charPosition[index] = position
      console.log(this.charPosition, 'push')
    },
  
    start (e) {
      e.preventDefault()
      const { pageX, pageY } = e.touches[0]
      Object.entries(this.charPosition).forEach(([key, value]) => {
        const { left, right, bottom, top } = value;
        if (left <= pageX && pageX <= right && top <= pageY && pageY <= bottom ) {
          this.touchStartIndex = parseInt(key);
        }
      })
    },

    move(e) {
      e.preventDefault()
      const { pageX, pageY } = e.touches[0]
      Object.entries(this.charPosition).forEach(([key, value]) => {
        const { left, right, bottom, top } = value;
        if (left <= pageX && pageX <= right && top <= pageY && pageY <= bottom ) {
          this.touchEndIndex = parseInt(key);
        }
      })
    },

    end (e) {
      e.preventDefault()
      if (this.touchStartIndex !== -1 && this.touchEndIndex !== -1) {
        const selectedChar = this.corpus.slice(this.touchStartIndex, this.touchEndIndex+1)
        if (selectedChar.every((item) => item.type === 0))
        this.$emit('selectWord', this.touchStartIndex, this.touchEndIndex)
      }
      this.touchStartIndex = this.touchEndIndex = -1
      this.charPosition = {}
    },
    
    recoverWord(index) {
      this.$emit('recoverWord', index)
    }
  }
}
</script>

<style lang="less" scoped>
.container{
  box-sizing: border-box;
  width: 100%;
  padding: 50rpx;
}
</style>