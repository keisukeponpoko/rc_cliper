<template lang="pug">
  div
    .contents
      p.title RoomClipチャート
      nuxt-link.button(to="/swipe") スタート

</template>

<script>
export default {
  layout: 'simple'
};
</script>

<style lang="scss" scoped>
.contents {
  margin: 3rem auto;
  text-align: center;
}

.title {
  font-size: 20px;
  color: #000;
  margin: 8rem 0 12rem;
}

.button {
  color: #fff;
  background: red;
  font-size: 18px;
  border-radius: 0.8rem;
  padding: 1.6rem;
  text-decoration: none;

  &:hover {
    opacity: 0.6;
  }
}
</style>
