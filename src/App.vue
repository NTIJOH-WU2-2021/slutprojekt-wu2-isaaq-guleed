// todos
// "Async created" was here before, does it really have to be used?? Await was also used
// Fix delay between body and themer div
<template>
<div id="themer" :class="{'theme-dark': lightMode}">
    <div class="toggles">
      <i class="fas fa-sun sun" v-if="lightMode" @click="lightMode = !lightMode"></i>
      <i class="fas fa-moon" v-else @click="lightMode = !lightMode"></i>
    </div>
    <div class="centered">
      <div v-if="!Object.keys(gif).length">
        <h3 class="flex flex-box justify-center">loading...</h3>
      </div>
      <div v-else class="flex">
        <img :src="gif.images.original.url" alt="" class="display-img" id="" />
      </div>
      <div class="buttons">
        <button class="text-6xl btn right" @click="swipeRight">✔️</button>
        <button class="text-6xl btn left" @click="swipeLeft">❌</button>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash';
import './assets/styles/index.css';
import { GiphyFetch } from '@giphy/js-fetch-api';

export default {
  name: 'vue-giphy',

  data() {
    return {
      likedGifs: [],
      dislikedGifs: [],
      gif: {},
      dark: false,
      root: null,
      theme: '',
      lightMode: false,
    };
  },
  watch: {
    lightMode() {
      localStorage.setItem('lightMode', JSON.stringify(this.lightMode));
    },
  },
  created() {
    this.loadRandomGif();
    this.lightMode = JSON.parse(localStorage.getItem('lightMode'));
  },
  methods: {
    async loadRandomGif() {
      const gf = new GiphyFetch('cK7HKf9DDVRtt6wCHMGR6Ls8Yov8byPm');
      const { data: gif } = await gf.random({ tag: 'meme' });
      this.gif = gif;
      console.log(gif);
    },

    gifIsNotSaved(gifArray, currentGifId) {
      return !_.some(gifArray, (gif) => gif.id === currentGifId);
    },

    async swipeRight() {
      if (this.gifIsNotSaved(this.likedGifs, this.gif.id)) {
        this.likedGifs.push(this.gif);
      }
      await this.loadRandomGif();
    },

    async swipeLeft() {
      if (this.gifIsNotSaved(this.dislikedGifs, this.gif.id)) {
        this.dislikedGifs.push(this.gif);
      }
      await this.loadRandomGif();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style lang="scss">
@import url(https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css);

body {
  margin: 0;
  padding: 0;
}
#themer {
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #23272A;
  color: white;
  position: absolute;
  transition: all .2s;

  i {
    position: absolute;
    top: 1em;
    left: 1em;
    font-size: 50px;
  }

  .sun {
    color: #ffe65b;
  }

  &.theme-dark {
    color: #333;
    background-color: white;
  }
}

</style>
