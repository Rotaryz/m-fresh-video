<template>
  <div class="video-container">
    <div class="container-bc">
      <img class="bc-img" src="./c-bc@2x.png" alt="">
    </div>
    <div class="content">
      <div class="content-header">
        <div class="header-tabs">
          <div v-for="(item, idx) in tabList" :key="idx" class="tab" :class="{'active' : item.type === selected}" @click="changeTab(item)">
            <img class="tab-icon" :src="item.icon" alt="">
            <p class="tab-txt">{{item.txt}}</p>
          </div>
        </div>
      </div>
      <div class="video-content">
        <div v-for="(item, idx) in arr[selected].list" :key="idx" class="video-item">
          <div class="item-header">
            <span class="item-circle">{{idx * 1 + 1}}</span>
            <p class="item-title">{{item.txt}}</p>
          </div>
          <div class="item-video-box">
            <img class="video-bc" src="./pic-playbox@2x.png" alt="">
            <div class="video-box">
              <!--<video :id="item.type" class="video" :src="item.url"
                     playsinline
                     x5-video-player-type="h5"
                     x5-video-orientation="landscape"
                     x5-video-player-fullscreen
                     controls
                     preload
                     :poster="item.poster"
                     @play="playVideo(item)"
              >
              </video>-->
              <div :id="item.type" class="video">
                <div :id="item.type" class="video-content"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import DPlayer from 'dplayer'

  const PAGE_NAME = 'VIDEO'

  const LIST = {
    mall: {
      list: [
        {
          txt: '业务流程',
          type: 'mall-1',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8e9a56a5285890790842434700/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8e9a56a5285890790842434700/1561648206_2098156387.100_0.jpg"
        },
        {
          txt: '拓展活动',
          type: 'mall-2',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ea192d5285890790842435171/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ea192d5285890790842435171/1561648206_569702460.100_0.jpg"
        },
        {
          txt: '营销活动',
          type: 'mall-3',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ea29f25285890790842435554/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ea29f25285890790842435554/1561648206_2086020895.100_0.jpg"
        }
      ]
    },
    supply: {
      list: [
        {
          txt: '业务流程',
          type: 'supply-1',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eaa8eb5285890790842436204/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eaa8eb5285890790842436204/1561648206_104897135.100_0.jpg"
        },
        {
          txt: '数字化',
          type: 'supply-2',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eabe325285890790842436706/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eabe325285890790842436706/1561648206_1260020087.100_0.jpg"
        },
        {
          txt: '移动化',
          type: 'supply-3',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b702ac755285890790842397505/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b702ac755285890790842397505/1561648206_2088012967.100_0.jpg"
        }
      ]
    },
    data: {
      list: [
        {
          txt: '数据图谱',
          type: 'data-1',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eb4ad15285890790842437722/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8eb4ad15285890790842437722/1561648206_3136762983.100_0.jpg"
        },
        {
          txt: '选品数据',
          type: 'data-2',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ebc6f85285890790842438386/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ebc6f85285890790842438386/1561648207_1236589795.100_0.jpg"
        },
        {
          txt: '社群数据',
          type: 'data-3',
          url: 'https://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ebdf735285890790842438903/v.f40.mp4',
          poster: "http://1255605079.vod2.myqcloud.com/8084de4fvodtransgzp1255605079/b8ebdf735285890790842438903/1561648206_97281551.100_0.jpg"
        }
      ]
    }
  }

  const TABS = [
    {txt: '商城系统', icon: './imgs/icon-mall@2x.png', type: 'mall'},
    {txt: '供应链系统', icon: './imgs/icon-supplychain@2x.png', type: 'supply'},
    {txt: '数据系统', icon: './imgs/icon-data@2x.png', type: 'data'},
  ]

  export default {
    name: PAGE_NAME,
    page() {
      return {
        title: '赞播优鲜'
      }
    },
    data() {
      return {
        arr: LIST,
        tabList: TABS,
        selected: 'mall',
        videoPlayers: {}
      }
    },
    mounted() {
      this.initVideo()
    },
    methods: {
      initVideo() {
        this.arr[this.selected].list.forEach(item => {
          let player = new DPlayer({
            container: document.getElementById(item.type),
            video: {
              url: item.url,
              pic: item.poster
            }
          })
          this.$set(this.videoPlayers, item.type, player)
        })
      },
      changeTab(item) {
        this.selected = item.type
        setTimeout(() => {
          this.initVideo()
        }, 200)
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~@design"

  .video-container
    width: 100vw
    height: 100vh
    position: relative
    background: #798DF4
    .container-bc
      width: 100%
      position: absolute
      left: 0
      top: 0
      font-size: 0
      .bc-img
        width: 100%
    .content
      width: 100vw
      height: 100vh
      position: absolute
      left: 0
      top: 0
      .content-header
        width: 100vw
        height: 77.73vw
        position: relative
        .header-tabs
          position: absolute
          left: 0
          bottom: 0
          display: flex
          height: 24.4vw
          width: 100vw
          padding: 0 3.4vw
          box-sizing: border-box
          .tab
            margin-right: 3.7vw
            flex: 1
            overflow: hidden
            display: flex
            flex-direction: column
            align-items: center
            justify-content: center
            background: white
            border-radius: 4px 4px 0 0
            &.active
              background: #798DF4
              .tab-txt
                color: white
            .tab-icon
              width: 9vw
              height: 9vw
              margin-bottom: 3vw
            .tab-txt
              font-family: PingFang-SC-Bold
              font-size: 14px
              color: #5568C5
              letter-spacing: 0
            &:last-child
              margin-right: 0


      .video-content
        width: 100vw
        background: #798DF4
        padding: 0 3.4vw 30px
        box-sizing: border-box
        .video-item
          padding-top: 20px
          .item-header
            display: flex
            align-items: center
            margin-bottom: 7px
            .item-circle
              width: 16px
              height: 16px
              line-height: 16px
              text-align: center
              font-family: PingFangSC-Regular
              font-size: 13px
              color: #798DF4
              background: white
              border-radius: 50%
              margin-right: 5px
            .item-title
              font-family: PingFangSC-Regular
              font-size: 13px
              line-height: 16px
              color: #FFFFFF
          .item-video-box
            width: 100%
            font-size: 0
            box-shadow: 0 2px 36px 0 #6478DF
            position: relative
            .video-bc
              width: 100%
            .video-box
              position: absolute
              left: 50%
              top: 50%
              transform: translate(-50%, -50%)
              width: 93.5%
              height: 0
              padding-bottom: 54.1%
              background: #000
              .video
                position: absolute
                left: 0
                top: 0
                width: 100%
                height: 100%
                .video-content
                  position: relative
                  width: 100%
                  height: 100%
  .test-page1
    width: 100%
</style>
