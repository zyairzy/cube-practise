<template>
  <div id="app">
    <div class="header">
      <div class="title" @click="showPicker">
        <span>全部赛事</span>
        <i class="cubeic-select" :class="{down: toDown}"></i>
      </div>
      <div class="navigator">
        <ul class="nav-list">
          <li
            v-for="(item, index) in tabList"
            :key="index"
            @click="switchTab(index)"
            :class="{active: currentPage === index}"
          >{{ item }}</li>
        </ul>
        <div class="triangle-up" :class="{left: currentPage === 0, right: currentPage === 2}"></div>
      </div>
    </div>
    <div class="content">
      <cube-slide
        :data="tabList"
        :initialIndex="currentPage"
        :loop="false"
        :autoPlay="false"
        :threshold="0.1"
        @change="slideChange"
      >
        <cube-slide-item v-for="(item, index) in tabList" :key="index">
          <div class="match-list-wrapper">
            <match-list :type="type" :status="index"></match-list>
          </div>
        </cube-slide-item>
        <div slot="dots"></div>
      </cube-slide>
    </div>
  </div>
</template>

<script>
import MatchList from './components/MatchList'
export default {
  name: 'app',
  data () {
    return {
      currentPage: 1,
      tabList: ['已结束', '直播中', '我的关注'],
      type: 'soccer',
      pickerList: [
        { text: 'NBA', value: 'NBA' },
        { text: 'DOTA', value: 'dota' },
        { text: 'SOCCER', value: 'soccer' }
      ],
      toDown: false
    }
  },
  components: {
    MatchList
  },
  mounted () {
    this.picker = this.$createPicker({
      title: '赛事',
      data: [this.pickerList],
      onValueChange: (selValue) => {
        this.type = selValue[0]
      },
      onSelect: () => {
        this.toDown = false
      },
      onCancel: () => {
        this.toDown = false
      }
    })
  },
  methods: {
    switchTab (index) {
      this.currentPage = index
    },
    slideChange (index) {
      this.currentPage = index
    },
    showPicker () {
      this.toDown = true
      this.picker.show()
    }
  }
}
</script>

<style lang="stylus">
html, body, #app {
  height: 100%;
  text-align: center;
}

#app {
  background-color: #E0E4E8;

  .header {
    color: white;
    background-color: #15191D;

    .title {
      padding: 20px 0;
      font-size: 16px;
      color: white;

      .down {
        display: inline-block
        transform: rotate(180deg)
      }
    }

    .navigator {
      position: relative;
      padding-bottom: 12px;
      font-size: 12px;

      .nav-list {
        display: flex;
        justify-content: space-around;

        li {
          width: 60px;
          color: #636873;

          &.active {
            color: white;
          }
        }
      }

      .triangle-up {
        position: absolute;
        left: 50%;
        transform: translate(-50%, 0);
        bottom: 0;
        width: 0;
        height: 0;
        border-left: 7px solid transparent;
        border-right: 7px solid transparent;
        border-bottom: 8px solid #E0E4E8;
        transition: all 0.4s;

        &.left {
          left: 16.67%;
        }

        &.right {
          left: 83.34%;
        }
      }
    }
  }

  .content {
    height: calc(100% - 80px);
    overflow: hidden;

    .match-list-wrapper {
      height: 100%;
    }
  }
}
</style>