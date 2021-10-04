<template>
  <div class="popup">
    <div class="popup-inner">
      <slot />
      <button class="popup-inner-close" @click="togglePopup()">
        x
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    togglePopup: {
      type: Function,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
.popup{
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.2);
  font-family: "Reenie Beanie";

  &-inner{
    background: #fff;
    border-radius: 12px;
    padding: 32px;
    position: relative;

    &-close{
      position: absolute;
      width: 24px;
      height: 24px;
      top: 0;
      right: 0;
      margin: 10px 10px 0 0;
      background-color: #ccc;
      border-radius: 50%;
      border: none;
      cursor: pointer;
    }
  }
}

@media screen and (max-width: 568px) {
  .popup{
    &-inner{
      max-width: 280px;
    }
  }
}
</style>
