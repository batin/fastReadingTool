<template>
  <div class="editButton">
    <Edit />
  </div>
</template>

<script>
import Edit from "@/assets/edit.svg?inline";
export default {
  components: { Edit },
  props: {
    onEdit: { type: Function }
  }
};
</script>

<style lang="scss">
.editButton {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  top: 12.5vh;
  background-color: #ff3e6c;
  right: 10vh;
  width: 50px;
  height: 50px;
  svg {
    width: 32px;
    height: 32px;
  }
}

@media screen and (max-width: 768px) {
  .editButton {
    top: 20vh;
    right: 2vh;
    width: 40px;
    height: 40px;
    svg {
      width: 25px;
      height: 25px;
    }
  }
}

@media screen and (max-width: 568px) {
  .editButton {
    top: 20vh;
    right: 2vh;
    width: 30px;
    height: 30px;
    svg {
      width: 20px;
      height: 20px;
    }
  }
}
</style>
