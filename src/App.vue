<template>
  <div id="app">
    <header>
      <h1>Traccy00</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="song-image">
          <img :src="current.image" alt="" />
        </div>
        <div class="controls">
          <button class="prev" @click="prev">
            <i class="fas fa-step-forward"></i>
          </button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Nang tho",
          artist: "Amee",
          src: require("./assets/nang-tho.mp3"),
          image: require("./assets/amee.jpg"),
        },
        {
          title: "Thang Tu",
          artist: "Ha Anh Tuan",
          src: require("./assets/thang-tu.mp3"),
          image: require("./assets/ha-anh-tuan.jpg"),
        },
        {
          title: "TA",
          artist: "Bất Thị Hoa Hỏa Nha",
          src: require("./assets/ta-bthhn.mp3"),
          image: require("./assets/ha-anh-tuan.jpg"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
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
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Shrikhand&family=Tourney:wght@100&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Shrikhand", sans-serif;
  overflow-y: scroll;
}
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  background: #fff;
}
::-webkit-scrollbar-thumb {
  border-radius: 15px;

  background: #cc2e50;
}
::-webkit-slider-thumb:hover {
  background: #ce1039;
}
.player {
  margin: 0 auto;
}
.song-image {
  width: 207px;
  height: 207px;
  margin: 0 auto;
  border-radius: 50%;
}
img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background: #000;
  color: #fff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #000;
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
  padding: 30px 15px;
  align-items: center;
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

.play,
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  color: #fff;
  background: #cc2e50;
  border-radius: 8px;
}
.next i,
.prev i {
  /* font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0 15px;
  border-radius: 6px;
  color: #fff;
  background: #ff5858; */
  color: #000;
}
.playlist {
  padding: 0 30px;
}

.playlist h3 {
  color: #212121;
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
}
.playlist .song:hover {
  color: #ff5858;
}
.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right, #cc2e50, #ff5858);
}
</style>
