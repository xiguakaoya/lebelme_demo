<template>
  <div class="word-contain">
    <span v-if="isChar" class="char">{{word.content}}</span>
    <span v-else :class="labelClass">
      <span>{{word.content}}</span>
      <icon type="clear" size="20" @click="recoverWord" />
    </span>
  </div>
</template>

<script>
export default {
  props:{
      word: {
        type: Object,
        required: true
      },
      index: {
        type: Number,
        required: true
      }
  },
  updated () {
    this.upLocation()
  },
  mounted () {
    this.upLocation()
  },
  computed: {
    isChar () {
      if (this.word.type === 0) {
        return true
      } else {
        return false
      }
    },
    labelClass () {
      const className = [];
      switch (this.word.type) {
        case 1:
          className.push('red');
          break;
        case 2:
          className.push('green')
          break;
        case 3:
          className.push('blue');
          break;
        default:
      }
      return className
    },
  },
  methods:{
    recoverWord () {
      this.$emit('recoverWord', this.index)
    },

    upLocation() {
    const charPosition = {};
    const query = uni.createSelectorQuery().in(this)
    query.select('.char').boundingClientRect((rect) => {
      if (rect) {
        this.$emit('getCharPosition', this.index, {
        left: rect.left,
        right: rect.right,
        top: rect.top,
        bottom: rect.bottom
      })
      }
    }).exec()
  }
  }
}
</script>

<style lang="less" scoped>
.red {
  background-color: red;
}
.green {
  background-color: green;
}
.blue {
  background-color: blue;
}
.word-contain{
  display: inline-block;
}
</style>