<template>
  <div id="app">
    <header>
      <h1>Hot Beatz!</h1>
    </header>
    <main>
      <hr>
      <section class="player">
        <h2 class="songTitle">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <hr>
      <section class="playlist">
        <h3>Playlist</h3>
        <hr>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
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
          title: 'Bustin Out',
          artist: 'GRiZ',
          src: require('./assets/bustin-out-griz.mp3')
        },
        {
          title: 'Fight Night',
          artist: 'Migos',
          src: require('./assets/fight-night-migos.mp3')
        },
        {
          title: 'Hit Yo Dance',
          artist: 'Rubi Rose',
          src: require('./assets/hit-yo-dance-rubi-rose.mp3')
        },
        {
          title: 'Juicy',
          artist: 'Doja Cat',
          src: require('./assets/juicy-doja-cat.mp3')
        },
        {
          title: 'Lotus Flower Bomb',
          artist: 'Wale',
          src: require('./assets/lotus-flower-bomb-wale.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;    
      }

      this.player.play();
      this.player.addEventListener('ended', function() {
        this.next();
      });
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
  * {
      margin: 0px;
      padding: 0px;
      box-sizing: border-box;
    }

  #app {
    background-image: linear-gradient(to bottom right, #111d5e, #c70039);
    height: 100vh;
  }

  body {
    font-family: sans-serif;
  }

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 15px;
    padding-top: 75px;
    color: #c70039;
    font-family: 'Fredoka One', cursive;
    letter-spacing: 3px;
  }

  hr {
  border: dotted #111d5e 6px;
  border-bottom: none;
  width: 15%;
  margin: 25px auto;
  }

  main {
    width: 100%;
    max-width: 768px;
    margin: 0px auto;
    padding: 25px;
  }

  .songTitle {
    color: #ffbd69;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
    padding-top: 25px;
    font-family: 'Lemonada', cursive;
  }

  .songTitle span {
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
    font-family: 'Noto Sans SC', sans-serif;  
  }

  button:hover {
    opacity: 0.65;
    transition: 0.5s ease;
  }

  .play, 
  .pause {
    font-size: 20px;
    font-weight: 700;
    margin: 0px 15px;
    padding: 10px 20px;
    border-radius: 30px;
    color: #111d5e;
    background-color: #c70039;
  }

  .next,
  .prev {
    font-size: 16px;
    font-weight: 700;
    margin: 0px 15px;
    padding: 5px 15px;
    border-radius: 20px;
    color: #111d5e;
    background-color: #f37121;
  }

  .playlist {
    padding: 0px 30px;
    font-family: 'Fredoka One', cursive;
  }

  .playlist h3 {
    color: #ffbd69;
    font-size: 28px;
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
    color: #ffbd69;
    font-family: 'Noto Sans SC', sans-serif;  
    }
  
  .playlist .song:hover {
    color: #f37121;
    transition: 0.5s ease;
  }

  .playlist .song.playing {
    color: #111d5e;
    background-image: linear-gradient(to right, #c70039, #f37121);
    border-radius: 25px;
  }

</style>