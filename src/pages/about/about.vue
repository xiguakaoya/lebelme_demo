<template>
  <div>
    <view class="desc-container">
      <view class="word">标签:</view>
      <view class="btn red" @click="()=>changeState(1)">红色</view>
      <view class="btn green" @click="()=>changeState(2)">绿色</view>
      <view class="btn blue" @click="()=>changeState(3)">蓝色</view>
    </view>
    <div class="select-result">
      <div class="dsc">当前选中的标签：</div>
      <div :class="selectColorClass"></div>
    </div>
    <WordSelect
      :corpus="corpus"
      @selectWord="selectWord"
      @recoverWord="recoverWord"
    />
  </div>
</template>

<script>
import WordSelect from './components/display/WordSelect'

export default {
  components:{
    WordSelect,
  },
  data(){
    return{
      selectLabelState: 1,
      location: null,
      corpus: [
        {'content':"段",'type': 0},
        {'content':"待",'type': 0},
        {'content':"标",'type': 0},
        {'content':"注",'type': 0},
        {'content':"的",'type': 0},
        {'content':"文",'type': 0},
        {'content':"本",'type': 0},
        {'content':"段",'type': 0},
        {'content':"待",'type': 0},
        {'content':"标",'type': 0},
        {'content':"注",'type': 0},
        {'content':"的",'type': 0},
        {'content':"文",'type': 0},
        {'content':"本",'type': 0},
        {'content':"段",'type': 0},
        {'content':"待",'type': 0},
        {'content':"标",'type': 0},
        {'content':"注",'type': 0},
        {'content':"的",'type': 0},
        {'content':"文",'type': 0},
        {'content':"本",'type': 0},
        {'content':"段",'type': 0},
        {'content':"待",'type': 0},
        {'content':"标",'type': 0},
        {'content':"注",'type': 0},
        {'content':"的",'type': 0},
        {'content':"文",'type': 0},
        {'content':"本",'type': 0}
      ]
    }
  },
  computed: {
    selectColorClass () {
      const className = ['color-container'];
      switch (this.selectLabelState) {
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
    }
  },

  methods: {
    changeState (labelState) {
      this.selectLabelState = labelState
    },
    recoverWord (index) {
      const charArr = Array.from(this.corpus[index].content).map((char) => ({ content: char, type: 0 }));
      const preArr = this.corpus.slice(0, index);
      const afArr = this.corpus.slice(index+1);
      this.corpus = [...preArr, ...charArr, ...afArr]
    },
    selectWord (startIndex, endIndex) {
      const labelArr = this.corpus.slice(startIndex, endIndex + 1)
      const labelStr = labelArr.reduce((acc, item) => {
        return acc+item.content; 
      }, '');
      const label = { content: labelStr, type: this.selectLabelState }
      this.corpus.splice(startIndex, endIndex - startIndex + 1, label)
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
  .desc-container{
		box-sizing: border-box;
    width: 100%;
    padding: 25rpx 50rpx;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    align-items: center;
    .word{
      margin-bottom: 25rpx;
    }
    .btn{
			width: 100rpx;
      height: 50rpx;
      line-height: 50rpx;
      text-align: center;
      margin-left: 50rpx;
      margin-bottom: 25rpx;
    }
  }
  .select-result{
     width: 100%;
     display: flex;
     justify-content: flex-end;
     align-items: center;
     margin-top: 10rpx;
     margin-left: 20rpx;
    .color-container{
      width: 100rpx;
      height: 50rpx;
    }

    .select-container{
      margin: 50rpx;
    }
  }
</style>
