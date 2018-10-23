<template lang="pug">
  div
    .contents
      p.title あなたにおすすめのインテリアは・・・

      p.result {{ text }}

      ul.images
        li.image(v-for='image in images')
          img.img(:src="'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/' + image + '.jpg'")

      p.explain
        | ホワイトインテリアとは、ホワイトなインテリアのことです。

      p.share 結果をシェア
      .shares
        button.twitter Twitter
        button.facebook Facebook

      .buttonBlock
        a.button(:href="'https://roomclip.jp/tag/' + id") {{ text }}の写真をもっと見る
</template>

<script>
export default {
  layout: 'simple',
  asyncData({ params }) {
    console.log(params);

    const TEXTS = [
      'ホワイトインテリア',
      'ナチュラルインテリア',
      '北欧インテリア',
      '西海岸インテリア',
      'シンプルインテリア',
      '男前インテリア',
      'モノトーンインテリア',
      'カフェ風インテリア',
      '塩系インテリア'
    ];

    const TAG_IDS = [
      '3x2100', '3x2296', '3x2517', '3x100672', '3x70903', '3x25144', '3x95766', '3x14339', '3x209607'
    ];

    if (!params.id) {
      return {
        text: 'あなたに合うインテリアを探してみよう！',
        id: '3'
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
      ]
    }
  }
};
</script>

<style lang="scss" scoped>
.contents {
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

.shares {
  display: flex;
  justify-content: center;

  button {
    color: #fff;
    font-size: 14px;
    border: none;
    border-radius: 0.8rem;
    padding: .4rem .8rem;
    margin: .2rem 1rem;
    text-decoration: none;
    width: 30%;
    height: 40px;
  }

  .twitter {
    background: #1da1f2;
  }

  .facebook {
    background: blue;
  }
}

.buttonBlock {
  margin-top: 3rem;
}

.button {
  color: #fff;
  background: #DC3C36;
  font-size: 14px;
  border-radius: 0.8rem;
  padding: .4rem .8rem;
  text-decoration: none;

  &:hover {
    opacity: 0.6;
  }
}

.share {
  margin: .5rem;
  display: flex;
  justify-content: center;

  .share_child {
    margin: .2rem;
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

.explain {
  margin: 2rem;
}

.result {
  font-size: 18px;
  font-weight: bold;
}

.img {
  width: 100%;
  height: 100%;
}
</style>
