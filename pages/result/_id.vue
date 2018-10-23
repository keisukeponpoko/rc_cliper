<template lang="pug">
  div
    #fb-root &nbsp;
    script
      | (function(d, s, id) {
      | var js, fjs = d.getElementsByTagName(s)[0];
      | if (d.getElementById(id)) return;
      | js = d.createElement(s); js.id = id;
      | js.src = "https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v3.0";
      | fjs.parentNode.insertBefore(js, fjs);
      | }(document, 'script', 'facebook-jssdk'));

    .contents
      p.title {{ text }}

      .share
        .share_child
          a.twitter-share-button(href="https://twitter.com/share?ref_src=twsrc%5Etfw" data-show-count="false") Tweet
            script(async src="https://platform.twitter.com/widgets.js" charset="utf-8")

        .share_child
          .fb-share-button(data-href="https://developers.facebook.com/docs/plugins/" data-layout="button_count" data-size="small" data-mobile-iframe="true")
            a.fb-xfbml-parse-ignore(target="_blank" href="https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fdevelopers.facebook.com%2Fdocs%2Fplugins%2F&amp;src=sdkpreparse")

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
        '73db0325d853452338f132f9ff109faff8b4bdbb',
        'c46c7505dbc318dd4924739a4973ec32a7a76cec'
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

.img {
  width: 100%;
  height: 100%;
}
</style>
