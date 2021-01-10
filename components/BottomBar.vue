<template>
  <div class="bottom-media-bar">
    <div class="media left">
      <div @click="() => onSplitSizeChange(-1)">-</div>
      {{ splitSize }}
      <div @click="onSplitSizeChange(1)">+</div>
    </div>
    <div class="media right">{{ index }} / {{ totalCount }}</div>
  </div>
</template>

<script>
export default {
  components: {},
  props: {
    onSplitSizeChange: { type: Function },
    index: { type: Number },
    totalCount: { type: Number },
    splitSize: { type: Number }
  }
};
</script>

<style lang="scss">
.bottom-media-bar {
  width: 40vh;
  position: absolute;
  bottom: 7.5vh;
  .media {
    display: flex;
    position: absolute;
    border-radius: 50px;
    padding: 1vh 5vh;
    color: white;
    font-weight: bold;
    background-color: #5f5fff;
    font-size: 24px;
    font-family: "Reenie Beanie";
    letter-spacing: 1.5px;
  }
  .left {
    left: 0;
    div {
      position: absolute;
      &:first-child {
        left: 10px;
      }
      &:last-child {
        right: 10px;
      }
    }
  }
  .right {
    right: 0;
    padding: 1vh 2vh;
  }
}
</style>
