<template>
  <div id="app">
    <header>
      <h1 class="">My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="control">
          <button class="prev"  @click="prev">Prev</button>
          <button class="play"  @click="play">Play</button>
          <button class="pause" @click="pause">Pause</button>
          <button class="next"  @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      songs: [
        {
          title: 'a-love-so-beautiful-ost-i-like-you-so-much-youll-know-pinyin-rom-eng',
          artist: 'Chinese Artist',
          src: require('./assets/a-love-so-beautiful-ost-i-like-you-so-much-youll-know-pinyin-rom-eng-lyrics.mp3')
        },
        {
          title: 'because-im-stupid-hangul-romanization-eng-sub',
          artist: 'Korean Artist',
          src: require('./assets/because-im-stupid-hangul-romanization-eng-sub.mp3')
        },
        {
          title: 'death-bed',
          artist: 'American Artist',
          src: require('./assets/death-bed.mp3')
        },
        {
          title: 'endless night',
          artist: 'Cambodian Artist',
          src: require('./assets/endless night.mp3')
        },
        {
          title: 'i-like-you-so-much-youll-know-it-lyrics-cover-by-ysabelle-cuevas-a-love-so-beautiful-ost.mp3',
          artist: 'American Artist',
          src: require('./assets/i-like-you-so-much-youll-know-it-lyrics-cover-by-ysabelle-cuevas-a-love-so-beautiful-ost.mp3')
        },
      ],
      player: new Audio()
    }
  },
  methods: {
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      // play the song
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      // play the song
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    play(song) {
      if(typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      // When the current song is ended, it will go to the next song
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        // play the song
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
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
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgb(41, 41, 41);
  color: white;
  padding: 1rem;
}
main {
  max-width: 48rem;
  margin: 0 auto;
  padding: 2rem;
}
.song-title {
  text-align: center;
  font-weight: 700;
  font-size: 2rem;
  text-transform: uppercase;
  color: black;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.control {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
}
.control button {
  margin: 0 0.5rem;
}.control button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 1.5rem;
  font-weight: 700;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  color: white;
  background-color: rgb(220, 0, 88);
  border: none;
}
.prev, .next {
  font-size: 1rem;
  font-weight: 700;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  color: white;
  background-color: rgb(255, 68, 0);
  border: none;
}
.playlist h3{
  padding-bottom: 1rem;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
}
.playlist .song {
  padding: 1rem;
  display: block;
  width: 100%;
  font-size: 1rem;
  font-weight: 700;
  border: none;
  background: white;
}
.playlist .song:hover {
  color: #FF5258;
}
.playlist .song.playing {
  color: white;
  background-image: linear-gradient(to right, #CC2E5D, #FF5258);
}


</style>
