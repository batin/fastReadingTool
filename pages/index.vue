<template>
  <div class="container">
    <div class="heading">
      <p>Fast Reading Tool</p>
    </div>
    <div class="reading-section">
      <p class="word">
        {{ currentWord }}
      </p>
    </div>
    <div v-if="!hasStarted" class="textbox">
      <Info class="info" />
      <textarea v-model="text" class="textarea" placeholder="" />
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
      :on-start="onStart"
      :on-jump="onJump"
      :is-playing="isPlaying"
      :speed="speed"
      :on-speed-change="onSpeedChange"
    />
  </div>
</template>

<script lang="ts">
import Vue from "vue"
import Writer from "@/assets/writer.svg?inline"
import Info from "@/assets/info.svg?inline"
import HomepageIcon from "@/assets/homepage-icon.svg?inline"
import MediaBar from "@/components/MediaBar.vue"

export default Vue.extend({
  components: {
    Writer,
    Info,
    HomepageIcon,
    MediaBar
  },

  data () {
    return {
      text: "" as string,
      currentWord: "" as string,
      dataSet: [] as string[],
      index: 0 as number,
      speed: 300 as number,
      hasStarted: false as boolean,
      isPlaying: false as boolean,
      currentInterval: 0 as any
    }
  },

  methods: {
    start () {
      if (this.text.length) {
        this.hasStarted = true
      }
      this.isPlaying = true
      this.splitText()

      this.currentInterval = setInterval(() => {
        if (this.isPlaying) {
          this.currentWord = this.dataSet[this.index]
          this.index++
        }
      }, this.speed)
    },
    splitText () {
      this.dataSet = this.text.split(" ")
    },

    onStart () {
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
      this.index += count
      this.currentWord = this.dataSet[this.index]
    }
  }
})
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Reenie+Beanie&display=swap");
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
}

@media screen and (min-width: 1280px) {
  .homepage-icon {
    display: block !important;
  }
}

@media screen and (max-width: 768px) {
  .container {
    .textbox {
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
  }
}
</style>
