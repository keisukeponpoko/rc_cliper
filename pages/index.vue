<template lang="pug">
  div
    .contents
      p.title あなたにおすすめのインテリアを探そう！

      p.text お気に入りの写真をいいねしてね！

      div
        img.img(src="https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/43575d9486982c9c5072856abd6dad5f840d62d6.jpg")

      div(style="margin: 2rem;")
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
  font-size: 18px;
  color: #000;
  margin: 1rem;
}

.img {
  width: 80%;
}

.text {
  font-size: 18px;
  color: #000;
  margin: 1rem;
}

.button {
  margin-top: 4rem;
  color: #fff;
  background: #DC3C36;
  font-size: 18px;
  border-radius: 0.8rem;
  padding: .8rem 1.6rem;
  text-decoration: none;
  font-weight: bold;

  &:hover {
    opacity: 0.6;
  }
}
</style>
