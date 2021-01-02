<template>
  <div class="media-bar">
    <div class="first-line">
      <FB10 @click="onJump(-10)" />
      <FB1 @click="onJump(-1)" />
      <Pause @click="onStop" v-if="isPlaying" />
      <Start @click="onStart" v-else />
      <FF1 @click="onJump(1)" />
      <FF10 @click="onJump(10)" />
    </div>
    <div class="second-line">
      <div class="progress-bar">
        <input
          type="range"
          id="volume"
          name="volume"
          step="1"
          :value="speed"
          min="100"
          max="2000"
          @change="onSpeedChange"
        />
        <div class="speed">
          {{ speed }}
        </div>
      </div>
      <Restart />
    </div>
  </div>
</template>

<script>
import FF10 from "@/assets/ff10.svg?inline";
import FF1 from "@/assets/ff1.svg?inline";
import FB10 from "@/assets/fb10.svg?inline";
import FB1 from "@/assets/fb1.svg?inline";
import Start from "@/assets/start.svg?inline";
import Pause from "@/assets/pause.svg?inline";
import Restart from "@/assets/restart.svg?inline";
export default {
  components: {
    FF10,
    FF1,
    FB10,
    FB1,
    Start,
    Pause,
    Restart
  },
  props: {
    onStart: { type: Function },
    onStop: { type: Function },
    onJump: { type: Function },
    onSpeedChange: { type: Function },
    isPlaying: { type: Boolean },
    speed: { type: Number }
  }
};
</script>

<style lang="scss">
.media-bar {
  position: absolute;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 50px;
  padding: 0 5vh;
  background-color: #ff3e6c;
  bottom: 10vh;
  .second-line {
    display: flex;
    width: 100%;
    justify-content: space-between;
    align-items: center;
    .speed {
      color: white;
      text-align: left;
      font-weight: bold;
      font-family: "Reenie Beanie";
      font-size: 24px;
      letter-spacing: 1px;
    }
    .progress-bar {
      width: 100%;
      margin-top: 30px;
      #volume {
        height: 20px;
        width: 100%;
      }
      svg {
        width: 95px;
      }
    }
  }
  svg {
    width: 80px;
  }
  svg:hover {
    cursor: pointer;
  }
}
@media screen and (max-width: 768px) {
  .media-bar {
    padding: 0 1vh;
    .second-line {
      width: 85%;
    }
    svg {
      width: 60px;
    }
  }
}

// progress bar css
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 22px;
  width: 22px;
  border-radius: 50%;
  margin-top: -5px;
  background-color: #5f5fff !important;
  cursor: pointer;
}

input[type="range"]::-webkit-slider-runnable-track {
  width: 100%;
  height: 12px;
  cursor: pointer;
  border-radius: 15px;
  box-shadow: none;
  background: white;
  border: 1px solid black;
}

input[type="range"] {
  -webkit-appearance: none; /* Hides the slider so that custom slider can be made */
  width: 100%; /* Specific width is required for Firefox. */
  background: transparent; /* Otherwise white in Chrome */
}

input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
}

input[type="range"]:focus {
  outline: none; /* Removes the blue border. You should probably do some kind of focus styling for accessibility reasons though. */
}

input[type="range"]::-ms-track {
  width: 100%;
  cursor: pointer;

  /* Hides the slider so custom styles can be added */
  background: transparent;
  border-color: transparent;
  color: transparent;
}
</style>
