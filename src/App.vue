<template>
<div id="themer" :class="{'theme-dark': lightMode}">
    <div class="toggles">
      <i class="fas fa-sun sun" v-if="lightMode" @click="lightMode = !lightMode"></i>
      <i class="fas fa-moon" v-else @click="lightMode = !lightMode"></i>
    </div>

    <div class="Title">
      <h1 class="title">Memeder</h1>
    </div>
    <section>
      <div class="wave2 water"></div>
      <div class="wave3 water"></div>
      <div class="wave water"></div>
    </section>

    <div class="centered">
      <div class="flex flex-col justify-center" id="fixed-layered">
        <div class="flex flex-row justify-center" id="fixed-layered">
          <button
            class="section-btn"
            @click="changeSection('tinder')"
          >
            Home
          </button>
          <button
            class="disliked-btn"
            @click="changeSection('disliked')"
          >
            Disliked
          </button>
          <button
            class="liked-btn"
            @click="changeSection('liked')"
          >
            Liked
          </button>
        </div>
      </div>
      <div v-if="section === 'liked'" class="overflow-scroll flex flex-col">
        <div v-if="likedGifs.length === 0">
          <h2 class="no-liked">
          No Liked Gifs😢</h2>
        </div>
        <div
        v-else
        v-for="likedGif in likedGifs.slice().reverse()"
        v-bind:key="likedGif"
        >
          <a :href="likedGif.images.original.url">
            <img :src="likedGif.images.original.url" class="display-img2">
          </a>
        </div>
      </div>
      <div v-else-if="section === 'disliked'" class="overflow-scroll flex flex-col">
        <div v-if="dislikedGifs.length === 0">
          <h2 class="no-disliked">
          No Disliked Gifs😞</h2>
        </div>
        <div
        v-else
        v-for="dislikedGif in dislikedGifs.slice().reverse()"
        v-bind:key="dislikedGif"
        >
          <a :href="dislikedGif.images.original.url">
            <img :src="dislikedGif.images.original.url" class="display-img2">
          </a>
        </div>
      </div>
      <div v-else>
        <div v-if="!Object.keys(gif).length">
          <h3 class="flex flex-box justify-center">loading...</h3>
        </div>
        <div v-else class="flex">
          <img :src="gif.images.original.url" alt="" class="display-img" id="" />
        </div>
        <div class="buttons">
          <button class="text-6xl btn left" @click="swipeLeft">❌</button>
          <button class="text-6xl btn right" @click="swipeRight">✔️</button>
        </div>
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
      section: 'tinder',
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

    changeSection(section) {
      this.section = section;
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

  .sun {
    color: #ffe65b;
  }

  &.theme-dark {
    color: rgb(255, 255, 255);
    background-color: white;
  }

  .toggles {
    position: relative;
    top: 0.5em;
    left: 1em;
    font-size: 50px;
    width: 60px;
    height: 60px;
    z-index: 2;
  }
}

.liked-btn {
    margin: 40px;
    padding: 20px;
    border-radius: 5px;
    border: none;
    transition: 0.2s;
    background-color: #5bff69;
    font-size: 20px;
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.226);
}

.liked-btn:hover {
    border: 2px solid rgba(0, 0, 0, 0.205);
    font-size: 28px;
}

.disliked-btn {
    margin: 40px;
    padding: 20px;
    border-radius: 5px;
    border: none;
    transition: 0.2s;
    background-color: #FF665B;
    font-size: 20px;
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.226);
}

.disliked-btn:hover {
    border: 2px solid rgba(0, 0, 0, 0.205);
    font-size: 28px;
}

.section-btn {
    margin: 40px;
    padding: 20px;
    border-radius: 5px;
    border: none;
    transition: 0.2s;
    background: linear-gradient(90deg, rgba(255,88,100,1) 0%, rgba(255,159,91,1) 100%);
    font-size: 20px;
    box-shadow: 3px 3px 5px rgba(0, 0, 0, 0.226);
}

.section-btn:hover {
    border: 2px solid rgba(0, 0, 0, 0.637);
    font-size: 28px;
}

#fixed-layered {
  z-index: 90000;
  margin-top: 10em;
}

.no-liked {
  color: white;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  text-shadow: 1px 4px 4px rgba(0,0,0,0.6);
}

.no-disliked {
  margin-bottom: 30em;
  color: white;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  text-shadow: 1px 4px 4px rgba(0,0,0,0.6);
}

.title {
  position: absolute;
  top: 0.5em;
  right: 1em;
  background: linear-gradient(90deg, rgba(255,88,100,1) 0%, rgba(255,159,91,1) 100%);
  padding: 0.5em;
  border-radius: 4px;
  font-size: 28px;
  text-shadow: 1px 4px 4px rgba(0,0,0,0.6);
}

</style>
