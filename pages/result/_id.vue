<template lang="pug">
  div
    .contents
      p.title {{ text }}
      ul.images
        li.image(v-for='image in images')
          img.img(:src="'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/' + image + '.jpg'")

      .buttonBlock
        a.button(:href="'https://roomclip.jp/tag/' + id") もっと見る
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

    const TAG_IDS = [
      2100, 2296, 2517, 100672, 70903, 25144, 95766, 14339, 209607
    ];

    if (!params.id) {
      return {
        text: 'あなたに合うインテリアを探してみよう！',
        id: 3
      };
    }

    return {
      text: TEXTS[params.id],
      id: TAG_IDS[params.id]
    };
  },
  data() {
    return {
      images: [
        '8412b653a3af6c26fe73818a3f3542b7f356768d',
        'c12ad0dcca764283f3ef62edf994dc22b0f3e0df',
        '43575d9486982c9c5072856abd6dad5f840d62d6',
        '2305caf87a3f405b3fc785c204a2ce2a2975148e',
        '73db0325d853452338f132f9ff109faff8b4bdbb',
        'c46c7505dbc318dd4924739a4973ec32a7a76cec'
      ]
    }
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
  margin: 2rem 2rem 2rem;
}

.text {
  font-size: 16px;
  color: #000;
  padding: 1.2rem;
}

.buttonBlock {
  margin-top: 3rem;
}

.button {
  color: #fff;
  background: #DC3C36;
  font-size: 18px;
  border-radius: 0.8rem;
  padding: 1.6rem;
  text-decoration: none;

  &:hover {
    opacity: 0.6;
  }
}

.images {
  display: flex;
  width: 90%;
  flex-wrap: wrap;
}

.image {
  list-style: none;
  width: 50%;
  height: 50%;
  padding: 0.2px 1px;
}

.img {
  width: 100%;
  height: 100%;
}
</style>
