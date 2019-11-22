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
        <li v-for="(item,i) in matchList" :key="i" class="match-item">
          <div class="left-side">
            <img :src="item.hostLogoUrl" alt class="logo" />
            <p class="name">{{item.hostTeamName}}</p>
          </div>
          <div class="center-side">
            <p v-if="item.live" class="live" :class="{end : item.isEnd}">{{item.live}}</p>
            <p v-if="item.order" class="order">{{item.order}}</p>
            <p class="score">{{item.hostScore}} - {{item.guestScore}}</p>
            <p class="time">{{item.isEnd? "" : item.endTime}}</p>
          </div>
          <div class="right-side">
            <img :src="item.guestLogoUrl" alt class="logo" />
            <p class="name">{{item.guestTeamName}}</p>
          </div>
        </li>
      </ul>
    </cube-scroll>
  </div>
</template>

<script>

import matchData from '../common/data/match-list'

export default {
  name: 'match-list',
  props: ['type', 'status'],
  data () {
    return {
      matchList: matchData[this.type][this.status],
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
            match.push(this.matchList[index])
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
            match.push(this.matchList[index])
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
  height: 618px;
  background-color: #FFFFFF;

  .match-item {
    border-bottom: 1px solid #E4E4E4;
    padding: 10px 0;
    display: flex;
    justify-content: space-around;

    .left-side, .right-side {
      width: 80px;

      .logo {
        display: inline-block;
        width: 38px;
        height: 38px;
        margin-bottom: 7px;
      }

      .name {
        font-size: 14px;
      }
    }

    .center-side {
      width: 80px;
      font-size: 12px;

      .live {
        background-color: #3D8F29;
        color: white;
        line-height: 16px;
        padding: 3px 10px;
        border-radius: 25px;
        margin-bottom: 7px;
      }

      .end {
        background-color: grey;
      }

      .order {
        border: 1px #2F6220 solid;
        line-height: 16px;
        padding: 3px 10px;
        border-radius: 25px;
        margin-bottom: 7px;
      }

      .score {
        font-size: 14px;
        font-weight: 500;
        margin-bottom: 7px;
        position: relative;
      }

      .time {
        color: grey;
        line-height: 16px;
        text-align: center;
      }
    }
  }
}
</style>