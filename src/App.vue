<template>
  <div class="p-20">
    <button class="btn btn-primary mb-10" @click="show = !show">
      Cliquez !
    </button>
    <div id="container">
      <Transition
        appear
        :css="false"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
      >
        <p v-show="show" id="balle"></p>
      </Transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const show = ref(true);

function beforeEnter(el) {
  el.style.opacity = 1;
  el.style.top = 0;
  el.style.left = 0;
}

function enter(el, done) {
  let pos = 0;
  let dir = 'down';
  let intervalId = setInterval(() => {
    if (pos < 0) {
      clearInterval(intervalId);
      done();
    } else if (dir === 'up') {
      pos--;
      el.style.top = pos + 'px';
    } else if (pos === 280) {
      dir = 'up';
    } else {
      pos++;
      el.style.top = pos + 'px';
    }
  }, 5);
}

function leave(el, done) {
  let opacity = 1;
  const intervalId = setInterval(() => {
    if (opacity <= 0.1) {
      clearInterval(intervalId);
      done();
    }
    el.style.opacity = opacity;
    opacity -= opacity * 0.1;
  }, 100);
}
</script>

<style lang="scss">
@import './assets/scss/base.scss';
#container {
  width: 500px;
  height: 500px;
  position: relative;
  background: black;
}
#balle {
  position: absolute;
  width: 200px;
  height: 200px;
  margin-left: 150px;
  border-radius: 55%;
  background-image: -webkit-radial-gradient(
    65px 65px,
    farthest-side,
    #eeeeee 0%,
    #53cbf1 40%,
    #002245 100%
  );
}
</style>
