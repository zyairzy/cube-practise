<template>
  <div class="match-list">
    <cube-scroll
      ref="scroll"
      :data="matchList"
      :options="options"
      @pulling-down="onPullingDown"
      @pulling-up="onPullingUp"
    >
      <ul>
        <li v-for="(v,i) in matchList" :key="i" style="height:30px">{{v}}</li>
      </ul>
    </cube-scroll>
  </div>
</template>

<script>
export default {
  name: 'match-list',
  props: ['type', 'status'],
  data () {
    return {
      matchList: ['比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛2', '比赛1', '比赛3'],
      options: {
        scrollbar: {
          fade: true
        },
        pullDownRefresh: {
          threshold: 90,
          stop: 40,
          txt: '刷新成功'
        },
        pullUpLoad: {
          threshold: 100,
          txt: {
            more: '加载成功',
            noMore: '没有更多的比赛啦'
          }
        }
      }
    }
  },
  methods: {
    onPullingDown () {
      setTimeout(() => {
        if (Math.random() > 0.5) {
          let match = []
          for (let index = 5; index > 0; index--) {
            match.push('this.matchList[index]')
          }
          this.matchList.unshift(...match)
        } else {
          this.$refs.scroll.forceUpdate()
        }
      }, 1000)
    },
    onPullingUp () {
      setTimeout(() => {
        if (Math.random() > 0.5) {
          let match = []
          for (let index = 5; index < 10; index++) {
            match.push('bbb')
          }
          this.matchList = this.matchList.concat(match)
        } else {
          this.$refs.scroll.forceUpdate()
        }
      }, 1000)
    }
  }
}
</script>

<style lang="stylus">
.match-list {
  height: 418px;
  background-color: #E2E5EA;
}
</style>