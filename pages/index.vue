<template>
  <div class="container">
    <div class="heading">
      <p>Fast Reading Tool</p>
    </div>
    <div class="reading-section">
      <p class="word">
        {{ currentWords }}
      </p>
    </div>
    <div v-if="!hasStarted" class="textbox">
      <Info class="info" />
      <textarea
        v-model="text"
        class="textarea"
        placeholder="Enter your text here"
      />
      <div class="button" @click="start">
        Start
      </div>
    </div>
    <div v-if="!hasStarted" class="writer-wrapper">
      <Writer class="writer" />
    </div>
    <HomepageIcon v-if="!hasStarted" class="homepage-icon" />
    <MediaBar
      v-if="hasStarted"
      :on-stop="onStop"
      :on-play="onPlay"
      :on-jump="onJump"
      :is-playing="isPlaying"
      :speed="speed"
      :on-speed-change="onSpeedChange"
      :on-restart="onRestart"
    />
    <BottomBar
      v-if="hasStarted"
      :index="index"
      :total-count="dataSet.length"
      :split-size="splitSize"
      :on-split-size-change="onSplitSizeChange"
    />
    <EditButton v-if="hasStarted" :on-edit="onEdit" />
    <Popup v-if="popupTrigger" class="popup-text-wrapper" :toggle-popup="togglePopup">
      <p>
        Please do not leave the text field blank
      </p>
    </Popup>
  </div>
</template>

<script lang="ts">
import Vue from "vue"
import Writer from "@/assets/writer.svg?inline"
import Info from "@/assets/info.svg?inline"
import HomepageIcon from "@/assets/homepage-icon.svg?inline"
import MediaBar from "@/components/MediaBar.vue"
import BottomBar from "@/components/BottomBar.vue"
import Popup from "@/components/Popup.vue"

export default Vue.extend({
  components: {
    Writer,
    Info,
    HomepageIcon,
    MediaBar,
    BottomBar,
    Popup
  },

  data () {
    return {
      text: "" as string,
      currentWords: "" as string,
      dataSet: [] as string[],
      index: 0 as number,
      speed: 1300 as number,
      hasStarted: false as boolean,
      isPlaying: false as boolean,
      currentInterval: 0 as any,
      splitSize: 1 as number,
      popupTrigger: false as boolean
    }
  },
  mounted () {
    const text = sessionStorage.getItem("text")
    const splitSize = sessionStorage.getItem("splitSize")
    const index = sessionStorage.getItem("index")
    this.text = text || ""
    this.splitSize = splitSize ? Number(splitSize) : 1
    this.index = Number(index)
  },
  methods: {
    start () {
      if (this.text.length) {
        this.hasStarted = true
      }
      if (this.text.length === 0) {
        this.popupTrigger = true
      }
      if (this.index > this.dataSet.length) {
        this.index = 0
      }
      sessionStorage.setItem("text", this.text)
      this.splitText()
      this.isPlaying = true
      this.currentInterval = setInterval(() => {
        if (this.index >= this.dataSet.length) {
          this.isPlaying = false
        }
        if (this.isPlaying) {
          sessionStorage.setItem("splitSize", this.splitSize.toString())
          sessionStorage.setItem("index", this.index.toString())
          this.onJump(1)
        }
      }, 2000 - this.speed)
    },

    togglePopup () {
      this.popupTrigger = !this.popupTrigger
    },

    chunk (arr: Array<string>, chunkSize: number) {
      if (chunkSize <= 0) {
        this.splitSize = 1
      }
      const Arr = []
      for (let i = 0, len = arr.length; i < len; i += chunkSize) {
        Arr.push(arr.slice(i, i + chunkSize))
      }
      return Arr
    },

    splitText () {
      this.dataSet = this.chunk(
        this.text.split(" "),
        this.splitSize
      ).map((item: Array<string>) => item.join(" "))
    },

    onPlay () {
      this.isPlaying = true
    },

    onStop () {
      this.isPlaying = false
    },

    onSpeedChange (e: any) {
      clearInterval(this.currentInterval)
      this.speed = Number(e.target.value)
      this.start()
    },

    onJump (count: number) {
      if (this.index + count >= this.dataSet.length) {
        this.index = this.dataSet.length
      } else if (this.index + count < -1) {
        this.index = 0
      } else {
        this.index += count
      }
      this.currentWords = this.dataSet[this.index]
    },

    onSplitSizeChange (count: number) {
      this.splitSize += count
      this.splitText()
    },

    onRestart () {
      this.index = 0
      this.currentWords = this.dataSet[this.index]
    },

    onEdit () {
      sessionStorage.setItem("splitSize", "1")
      sessionStorage.setItem("index", "0")
      this.index = 0
      this.splitSize = 1
      this.isPlaying = false
      this.hasStarted = false
    }
  }
})
</script>

<style lang="scss">
@font-face {
  font-family: "Reenie Beanie";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("../assets/ReenieBeanie-Regular.ttf") format("woff2");
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA,
    U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215,
    U+FEFF, U+FFFD;
}
@keyframes slide-right-writer {
  from {
    left: -200px;
  }
  to {
    left: 20px;
  }
}

@keyframes slide-right-textbox {
  from {
    left: -200px;
  }
  to {
    left: 22vh;
  }
}

@keyframes slide-left {
  from {
    right: -200px;
  }
  to {
    right: 10vh;
  }
}

@keyframes breath {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.4);
  }
  100% {
    transform: scale(1);
  }
}

.container {
  position: relative;
  margin: 0 auto;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  overflow: hidden;

  .heading {
    font-size: 60px;
    line-height: 70px;
    align-self: start;
    margin-top: 20px;
    letter-spacing: 5px;
    color: #5f5fff;
    padding: 20px;
    font-weight: 200;
    font-family: "Reenie Beanie";
  }

  .writer-wrapper {
    position: absolute;
    bottom: 20px;
    left: 20px;
    .writer {
      height: 30vh;
      width: 30vh;
      bottom: 0;
    }
    animation-name: slide-right-writer;
    animation-duration: 1.5s;
  }

  .textbox {
    position: absolute;
    border: 1px solid #e4e4e4;
    border-radius: 10px;
    height: 45vh;
    width: 60vh;
    left: 22vh;
    bottom: 28vh;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.25);
    animation-name: slide-right-textbox;
    animation-duration: 1.5s;

    .info {
      position: absolute;
      right: 10px;
      top: 10px;
      width: 25px;
      height: 25px;
      animation-delay: 1.5s;
      animation: breath infinite 1.5s;
      cursor: pointer;
      path {
        fill: #5f5fff;
      }
    }

    .button {
      position: absolute;
      padding: 10px 15px;
      background-color: #ff3e6c;
      color: white;
      font-size: 20px;
      letter-spacing: 2px;
      border-radius: 5px 2px 10px 2px;
      box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.25);
      bottom: -1px;
      right: -1px;
      font-weight: bold;
      cursor: pointer;
      font-family: "Reenie Beanie";
    }
  }

  .textarea {
    position: absolute;
    height: 42vh;
    width: 50vh;
    left: 10px;
    border: none;
    top: 5px;
    font-size: 13px;
    margin: 5px;
    resize: none;
    &::-webkit-scrollbar {
      display: none;
    }
    &:focus {
      outline: none;
    }
    &::placeholder {
      font-family: "Reenie Beanie";
      font-size: 30px;
    }
  }

  .homepage-icon {
    display: none;
    position: absolute;
    right: 10vh;
    width: 30vw;
    height: 100%;
    animation-name: slide-left;
    animation-duration: 1.5s;
  }
  .reading-section {
    position: absolute;
    .word {
      font-size: 60px;
      line-height: 70px;
      letter-spacing: 5px;
      color: #030047;
      border-radius: 50px;
      font-weight: 200;
    }
  }
  .popup-text-wrapper{
    font-size: 32px;
  }
}

@media screen and (min-width: 1280px) {
  .homepage-icon {
    display: block !important;
  }
}

@media screen and (max-width: 768px) {
  .container {
    .textbox {
      animation-name: none;
      left: unset;
      height: 25vh;
      width: 40vh;
      bottom: 30vh;
      .info {
        right: 5px;
        top: 5px;
      }
      .textarea {
        height: 23vh;
        width: 30vh;
        font-size: 11px;
      }
      .button {
        padding: 7.5px 10px !important;
      }
    }
    .reading-section {
      position: absolute;
      .word {
        font-size: 40px;
        margin: 0 15px;
        padding-bottom: 50px;
      }
    }
  }
}
@media screen and (max-width: 568px) {
  .container {
    .heading {
      font-size: 35px;
    }

    .popup-text-wrapper{
      font-size: 26px;
  }
  }
  .container .reading-section .word {
    font-size: 30px;
    margin: 0 15px;
    padding-bottom: 50px;
  }
}
</style>
