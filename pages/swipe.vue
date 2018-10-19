<template lang="pug">
  div
    .contents
      Swiper(:queue="queue" @submit="submit")
</template>

<script>
import Swiper from '~/components/v2/question/Swiper.vue';

export default {
  layout: 'simple',
  components: { Swiper },
  data() {
    return {
      queue: [],
      types: {
        WHITE: 0,
        NATURE: 1,
        SCANDINAVIA: 2,
        WEST_COAST: 3,
        SIMPLE: 4,
        COOL: 5,
        MONOTONE: 6,
        CAFE: 7,
        SALT: 8
      },
      points: {},
      counts: 0
    };
  },
  computed: {
    popularType() {
      console.log(this.counts);
      if (this.counts == this.types.length || this.counts == 0) {
        return '';
      }

      let point = 0;
      let popularType = this.types.WHITE;
      console.log(this.points);
      for (let key of Object.keys(this.points)) {
        if (point < this.points[key]) {
          point = this.points[key];
          popularType = key;
        }
      }
      return popularType;
    }
  },
  created() {
    this.getData();
    for (let key of Object.keys(this.types)) {
      this.points[this.types[key]] = 0;
    }
  },
  methods: {
    getData() {
      const list = [
        {
          key: 1,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/8412b653a3af6c26fe73818a3f3542b7f356768d.jpg',
          points: {
            0: 1,
            1: 2,
            4: 7
          }
        },
        {
          key: 2,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/c12ad0dcca764283f3ef62edf994dc22b0f3e0df.jpg',
          points: {
            3: 7,
            5: 1,
            7: 2
          }
        },
        {
          key: 3,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/43575d9486982c9c5072856abd6dad5f840d62d6.jpg',
          points: {
            1: 2,
            2: 5,
            4: 3
          }
        },
        {
          key: 4,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/2305caf87a3f405b3fc785c204a2ce2a2975148e.jpg',
          points: {
            0: 2,
            1: 5,
            7: 3
          }
        },
        {
          key: 5,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/73db0325d853452338f132f9ff109faff8b4bdbb.jpg',
          points: {
            3: 8,
            5: 2
          }
        },
        {
          key: 6,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/c46c7505dbc318dd4924739a4973ec32a7a76cec.jpg',
          points: {
            0: 4,
            1: 4,
            4: 2
          }
        },
        {
          key: 7,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/c96d2601ed7b631a85641515d0b788330805f197.jpg',
          points: {
            5: 8,
            7: 2
          }
        },
        {
          key: 8,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/5b3990224599c325b686ef9903f87b1b10aabad3.jpg',
          points: {
            0: 5,
            4: 3,
            6: 2
          }
        },
        {
          key: 9,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/5c59775f8ded9c89cd5924b3aaecc67944d0d101.jpg',
          points: {
            1: 2,
            2: 4,
            7: 4
          }
        },
        {
          key: 10,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/d2d17ab101e3640dc77ea3b08d39896d16029172.jpg',
          points: {
            2: 2,
            6: 3,
            8: 5
          }
        },
        {
          key: 11,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/769ade8d6e11efca9a959dfe262d32b1bbe2fca4.jpg',
          points: {
            5: 3,
            7: 6,
            8: 1
          }
        },
        {
          key: 12,
          url:
            'https://cdn.roomclip.jp/v1/640/roomclip-bucket/img_640/50afadcb93044fdcdd99e982226ffc416ac36809.jpg',
          points: {
            4: 1,
            6: 5,
            8: 4
          }
        }
      ];
      this.queue = this.queue.concat(list);
    },
    submit({ type, item }) {
      if (this.queue.length <= 0) {
        this.$router.push('/result/' + this.popularType);
      }

      switch (type) {
        case 'nope': // 左
          break;
        case 'like': // 右
          for (let key of Object.keys(item.points)) {
            this.points[key] += item.points[key];
          }
          this.counts += 1;
          break;
        case 'super': // 上
          for (let key of Object.keys(item.points)) {
            this.points[key] += item.points[key] * 2;
          }
          break;
      }
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
  margin: 8rem 0 12rem;
}

.button {
  color: #fff;
  background: red;
  font-size: 18px;
  border-radius: 0.8rem;
  padding: 1.6rem;
  text-decoration: none;
  font-weight: bold;

  &:hover {
    opacity: 0.6;
  }
}
</style>
