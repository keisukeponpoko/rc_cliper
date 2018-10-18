<template lang="pug">
  div
    .contents
      p.title チャート結果
      p.text {{ text }}
        br
        | &nbsp;
      router-link(to="/") チャートTOPに戻る
</template>

<script>
export default {
  layout: 'simple',
  asyncData({ params }) {
    console.log(params);

    const TEXTS = [
      'あなたにはホワイトインテリアがおすすめ！',
      'あなたにはナチュラルインテリアがおすすめ！',
      'あなたには北欧インテリアがおすすめ！',
      'あなたには西海岸インテリアがおすすめ！',
      'あなたにはシンプルインテリアがおすすめ！',
      'あなたには男前インテリアがおすすめ！',
      'あなたにはモノトーンインテリアがおすすめ！',
      'あなたにはカフェ風インテリアがおすすめ！',
      'あなたには塩系インテリアがおすすめ！'
    ];

    return {
      text: TEXTS[params.id]
    };
  }
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

.text {
  font-size: 16px;
  color: #000;
  padding: 1.2rem;
}
</style>
