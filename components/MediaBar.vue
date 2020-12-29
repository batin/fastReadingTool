<template>
  <div class="media-bar">
    <FB10 @click="onJump(-10)" />
    <FB1 @click="onJump(-1)" />
    <Pause @click="onStop" v-if="isPlaying" />
    <Start @click="onStart" v-else/>
		
    <FF1 @click="onJump(1)" />
    <FF10 @click="onJump(10)" />
  </div>
</template>

<script>
import FF10 from "@/assets/ff10.svg?inline";
import FF1 from "@/assets/ff1.svg?inline";
import FB10 from "@/assets/fb10.svg?inline";
import FB1 from "@/assets/fb1.svg?inline";
import Start from "@/assets/start.svg?inline";
import Pause from "@/assets/pause.svg?inline";
import MediaButton from "../components/MediaButton";
export default {
  components: {
    FF10,
    FF1,
    FB10,
    FB1,
    Start,
    Pause
  },
	props: {
		onStart: { type: Function },
		onStop: { type: Function },
		onJump: { type: Function },
		isPlaying: { type: Boolean } 
	}
};
</script>

<style lang="scss">
.media-bar {
  position: absolute;
  display: flex;
  border-radius: 50px;
  padding: 0 5vh;
  background-color: #ff3e6c;
  bottom: 10vh;
  svg {
    width: 80px;
  }
  svg:hover {
    cursor: pointer;
  }
  @media screen and (max-width: 768px) {
    padding: 0 1vh;
    svg {
      width: 60px;
    }
  }
}
</style>
