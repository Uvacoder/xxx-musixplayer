<template>
  <div id="app">
    <header>
      <h1>Musix Player</h1>
    </header>

    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src) == (current.src) ? 'song playing' : 'song' ">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Love of My Life',
          artist: 'Ben Lvcas',
          src: require('../src/assets/Ben_Lvcas_-_Love_of_My_Life.mp3')
        },
        {
          title: 'Disconnect',
          artist: 'The Vow',
          src: require('../src/assets/The_Vow_-_Disconnect.mp3')
        }
      ],
      player: new Audio(),
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
       this.player.play();
       this.player.addEventListener("ended", function () {
        this.index++;
        if(this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
       }.bind(this));
       this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if(this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  }

  header {
    display: grid;
    place-items: center;
    padding: 20px;
    background-color: rgb(63, 26, 13);
    color: #eff6d8;
  }

  main {
    width: 100%;
    max-width: 768px;
    margin: 0 auto;
    padding: 25px;
  }

  .song-title{
    color: #212121;
    font-size: 32px;
        font-weight: 700;

    text-transform: uppercase;
    text-align: center;
  }
  .song-title span {
    font-weight: 400;
  font-style: italic;
  }

  .controls {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
  }

  button {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
  }
  button:hover {
    opacity: 0.8;
  }

  .play, .pause {
    font-size: 20px;
    font-weight: 700;
    padding: 15px 25px;
    margin: 0 15px;
    border-radius: 8px;
    color: white;
    background-color: #cc2e5d;
  }

  .next, .prev {
    font-size: 16px;
    font-weight: 700;
    padding: 10px 25px;
    margin: 0 15px;
    border-radius: 6px;
    color: white;
    background-color: #ff5858;
  }

  .playlist {
    padding: 0 30px;
  }
  
  .playlist h3 {
    color: #212121;
    font-size: 20px;
    font-weight: 400;
    margin-bottom: 30px;
    text-align: center;
  }

  .playlist .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
  }
  .playlist .song:hover {
    color: #ff5858;
  }

  .playlist .song.playing {
    color: #ffffff;
    background-image: linear-gradient(to right, #cc2e5d, #ff5858);
  }
</style>
