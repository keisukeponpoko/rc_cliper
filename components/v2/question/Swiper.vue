<template>
  <div style="width: 100%;">
    <no-ssr>
      <div class="swipe-wrapper">
        <div
          class="swipe"
          @touchstart="start"
          @touchmove="move"
          @touchend="end"
          @touchcancel="end"
          @mousedown="start"
          @mousemove="move"
          @mouseup="end"
          @mouseout="end">
          <transition-group
            @leave="leave"
            @afterLeave="gone">
            <div
              v-for="(item,index) in queue"
              v-if="index<2"
              :style="isActiveCard(index)?mainCardStyle():benchCardStyle(index)"
              :key="item[keyName]"
              class="swipe-card">
              <div
                :style="`background-image:url(${item.url})`"
                class="pic"/>
            </div>
          </transition-group>
        </div>
      </div>
      <div class="btns">
        <img
          src="~assets/swipe/nope.png"
          @click="decide('nope')">
        <!--<img-->
        <!--src="~assets/swipe/super-like.png"-->
        <!--@click="decide('super')">-->
        <img
          src="~assets/swipe/like.png"
          @click="decide('like')">
      </div>
    </no-ssr>
  </div>
</template>

<script>
export default {
  props: {
    queue: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      state: {
        touchId: null,
        start: {},
        move: {},
        startPoint: 1,
        ratio: 0,
        result: null
      },
      size: {
        top: 0,
        width: 0,
        height: 0
      },
      keyName: 'key',
      status: 0
    };
  },
  computed: {
    nowKey() {
      if (this.status === 2) {
        return null;
      }
      return this.queue[0] && this.queue[0][this.keyName];
    },
    pointerOpacity() {
      return this.state.ratio / 0.5;
    },
    likeOpacity() {
      if (this.superOpacity) {
        return 0;
      }
      return this.pointerOpacity;
    },
    nopeOpacity() {
      return -this.likeOpacity;
    },
    superOpacity() {
      return 0;
      // const disY = this.state.move.y - this.state.start.y;
      // const ratio = disY / (-0.5 * this.size.height);
      // return ratio > Math.abs(this.pointerOpacity) ? ratio : 0;
    }
  },
  mounted() {
    this.size = {
      top: this.$el.offsetTop,
      width: this.$el.offsetWidth,
      height: 350
    };
  },
  methods: {
    // 現在表示されているカードか判断する。
    isActiveCard(index) {
      return index === 0 && this.queue[index][this.keyName] === this.nowKey;
    },
    // 現在表示されているカードのスタイル
    mainCardStyle() {
      const style = { zIndex: 10, transform: '', transition: '' };
      if (this.status === 0) {
        style.transform = 'scale(1) translate3d(0,0,0) rotate(0deg)';
        style.transition = 'all 500ms cubic-bezier(0.175, 0.885, 0.32, 1.275)';
      } else if (this.status === 1) {
        const state = this.state;
        const { start, move, startPoint } = state;
        const x = move.x - start.x || 0;
        const y = move.y - start.y || 0;
        const ratio = x / (this.size.width * 0.5);
        state.ratio = ratio;
        const rotate = 10 * ratio * startPoint;
        style.transform = `translate3d(${x}px,${y}px,${y}px) rotate(${rotate}deg)`;
        style.transition = 'none';
      }
      return style;
    },
    // 後ろに位置するカードのスタイル
    benchCardStyle(index) {
      if ((index === 1 && this.status === 2) || index > 1) {
        return {
          zIndex: -1,
          opacity: 0
        };
      }
      const style = { zIndex: 9, transform: '', transition: '' };
      if (this.status === 0) {
        style.transform = 'scale3d(0.95,0.95,1)';
        style.transition = 'all 500ms ease';
      } else if (this.status === 1) {
        let ratio = this.state.ratio;
        if (ratio > 1) {
          ratio = 1;
        } else if (ratio < -1) {
          ratio = -1;
        }
        style.transform = `scale3d(${Math.abs(ratio) * 0.05 + 0.95},${Math.abs(
          ratio
        ) *
          0.05 +
          0.95},1)`;
        style.transition = 'none';
      } else if (this.status === 2) {
        style.transform = 'scale3d(1,1,1)';
        style.transition = 'all 500ms cubic-bezier(0.175, 0.885, 0.32, 1.275)';
      }
      return style;
    },
    // ボタン押した時の処理
    decide(type) {
      if (this.state.touchId || this.status !== 0 || !this.nowKey) {
        return;
      }
      this.state.start = { x: 0, y: 0 };
      this.state.move = {
        x: type === 'super' ? 0 : type === 'like' ? 1 : -1,
        y: type === 'super' ? -1 : 0
      };
      this.state.startPoint = 1;
      this.shiftCard(type);
    },
    start(e) {
      const state = this.state;
      if (state.touchId !== null || this.status === 2) {
        return;
      }
      let pageX;
      let pageY;
      if (e.type === 'touchstart') {
        pageX = e.changedTouches[0].pageX;
        pageY = e.changedTouches[0].pageY;
      } else {
        pageX = e.clientX;
        pageY = e.clientY;
      }
      const top = this.size.top;
      const height = this.size.height;
      const centerY = top + height / 2;
      const startPoint = pageY > centerY ? -1 : 1;
      this.status = 1;
      this.state = {
        touchId:
          e.type === 'touchstart' ? e.changedTouches[0].identifier : 'mouse',
        start: {
          x: pageX,
          y: pageY
        },
        move: Object.create(null),
        startPoint,
        ratio: 0,
        result: null
      };
    },
    move(e) {
      e.preventDefault();
      const state = this.state;
      if (
        state.touchId === null ||
        this.status === 2 ||
        (e.type === 'touchmove' &&
          state.touchId !== e.changedTouches[0].identifier)
      ) {
        return;
      }
      let pageX;
      let pageY;
      if (e.type === 'touchmove') {
        pageX = e.changedTouches[0].pageX;
        pageY = e.changedTouches[0].pageY;
      } else {
        pageX = e.clientX;
        pageY = e.clientY;
      }
      state.move = {
        x: pageX,
        y: pageY
      };
    },
    end(e) {
      if (
        e.type === 'touchstart' &&
        this.state.touchId !== e.changedTouches[0].identifier
      ) {
        return;
      }
      if (this.status === 2) {
        return;
      }
      if (Math.abs(this.pointerOpacity) >= 1 || this.superOpacity >= 1) {
        const result =
          this.superOpacity >= 1
            ? 'super'
            : this.pointerOpacity > 0
              ? 'like'
              : 'nope';
        this.shiftCard(result);
      } else {
        this.gone();
      }
    },
    // スワイプが足りなかった場合は、スワイプ前の位置にカードを戻す
    gone() {
      this.status = 0;
      this.state = {
        touchId: null,
        start: {},
        move: {},
        startPoint: 1,
        ratio: 0,
        result: null
      };
    },
    // 次のカードを表示する。
    shiftCard(type) {
      this.status = 2;
      this.state.result = type;
      const item = this.queue.shift();
      this.$emit('submit', { type, item });
    },
    // transion時
    leave(el, done) {
      const state = this.state;
      const { start, move, startPoint } = state;
      let x = move.x - start.x || 0;
      let y = move.y - start.y || 0;
      if (state.result === 'super') {
        y -= this.size.width;
      } else {
        x += this.size.width * (x < 0 ? -0.5 : 0.5);
        y *= x / (move.x - start.x);
      }
      const ratio = x / (this.size.width * 0.5);
      const rotate = (ratio / (0.8 / 0.5)) * 15 * startPoint;
      const duration =
        state.touchId === null || state.result === 'super' ? 800 : 300;
      el.className += ` ${state.result}`;
      el.style.opacity = 0;
      el.style.transform = `translate3d(${x}px,${y}px,0) rotate(${rotate}deg)`;
      el.style.transition = `all ${duration}ms ease`;
      setTimeout(done, duration);
    }
  }
};
</script>

<style lang="scss" scoped>
.swipe-wrapper {
  height: 420px;
  position: relative;
}
.swipe {
  position: absolute;
  z-index: 1;
  left: 0;
  right: 0;
  top: 0;
  margin: auto;
  min-width: 300px;
  max-width: 355px;
}
.swipe-card {
  position: absolute;
  height: auto;
  overflow: hidden;
  background: #fff;
  border-radius: 1rem;
  box-shadow: 0 0.2rem 1rem rgba(0, 0, 0, 0.15);
}
.v-move {
  transition: none !important;
}
.pointer-wrap {
  pointer-events: none;
  transition: opacity 0.2s ease;
}
.tinder-card.nope .nope-pointer-wrap,
.tinder-card.like .like-pointer-wrap,
.tinder-card.super .super-pointer-wrap {
  opacity: 1 !important;
}
.btns {
  margin: auto;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 300px;
  max-width: 355px;
}
.btns img {
  width: 80px;
}
.nope-pointer {
  right: 10px;
}
.like-pointer {
  left: 10px;
}
.nope-pointer,
.like-pointer {
  position: absolute;
  z-index: 1;
  top: 20px;
  width: 64px;
  height: 64px;
}
.super-pointer {
  position: absolute;
  z-index: 1;
  bottom: 80px;
  left: 0;
  right: 0;
  margin: auto;
  width: 112px;
  height: 78px;
}
.pic {
  width: 350px;
  height: 350px;
  background-size: cover;
}
</style>
