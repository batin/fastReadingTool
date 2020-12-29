<template> 
  <div>
    <div v-html={icon}></div>
  </div>
</template>

<script>
export default {
  props: {
    onClick: () => Function,
    icon: HTMLElement,
  }
};
</script>

<style></style>
