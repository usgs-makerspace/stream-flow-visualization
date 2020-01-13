<template>
  <div id="legend">
    <img class="img" src="../images/legend/image.png" alt="Legend" />
  </div>
</template>
<script>
export default {
  name: "legend"
};
</script>
<style scoped lang="scss">
    #legend{
        position: absolute;
        bottom: 10px;
        left: 10px;
        z-index: 1000;
    }
</style>