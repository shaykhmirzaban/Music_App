<template>
  <section class="center-div">
    <main class="mobile-structure">
      <!-- header -->
      <header>
        <div class="image">
          <img :src="current.image" alt="image not found">
        </div>
        <div class="content">
          <h1>{{ current.name }}</h1>
          <h4>{{ current.artist }}</h4>
        </div>
      </header>
      <!-- song lists -->
      <section class="songList">
        <table>
          <tr>
            <th>No</th>
            <th>Name</th>
            <th>Artist</th>
            <th>Time</th>
          </tr>
          <tr v-for="song in songs" :key="song.id">
            <td>{{ song.id }}</td>
            <td>{{ song.name }}</td>
            <td>{{ song.artist }}</td>
            <td>{{ song.duration }}</td>
            <td @click="play(song)"><i class="fa-solid fa-play"></i></td>
          </tr>
        </table>
      </section>
      <!-- bar -->
      <section class="bar">
        <div class="starting">00:00</div>
        <input type="range" min="0" max="100" value="0" id="range" />
        <div class="duration">{{ timeOfSong }}</div>
      </section>
      <!-- control section -->
      <section class="bottom">
        <div class="prev" @click="prev"><i class="fa-solid fa-backward"></i></div>
        <div class="play" @click="play" v-if="isActive"><i class="fa-solid fa-play"></i></div>
        <div class="pause" @click="pause" v-else><i class="fa-solid fa-circle-pause"></i></div>
        <div class="next" @click="next"><i class="fa-solid fa-forward"></i></div>
      </section>
    </main>
  </section>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      isActive: true,
      timeOfSong: "00:00",
      songs: [
        {
          name: "Glass",
          artist: "GalBoy",
          src: "./public/songs/one.mp3",
          image: "./public/images/a-1.jpg",
          duration: "02:47",
          id: 1
        },
        {
          name: "Cup",
          artist: "CpuBoy",
          src: "./public/songs/two.mp3",
          image: "./public/images/a-2.jpg",
          duration: "03:14",
          id: 2
        },
        {
          name: "Calculator",
          artist: "CalBoy",
          src: "./public/songs/three.mp3",
          image: "./public/images/a-3.jpg",
          duration: "01:55",
          id: 3
        },
        {
          name: "Clock",
          artist: "ClockBoy",
          src: "./public/songs/four.mp3",
          image: "./public/images/a-4.jpg",
          duration: "02:26",
          id: 4
        },
        {
          name: "Book",
          artist: "BookBoy",
          src: "./public/songs/five.mp3",
          image: "./public/images/a-5.jpg",
          duration: "02:34",
          id: 5
        },
        {
          name: "Key",
          artist: "KeyBoy",
          src: "./public/songs/six.mp3",
          image: "./public/images/d-5.jpg",
          duration: "02:36",
          id: 6
        }
      ],
      player: new Audio()
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.timeOfSong = this.current.duration;
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

        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);

      }.bind(this));

      // document element
      var start = document.querySelector(".starting");
      var myRange = document.querySelector("#range");

      this.player.addEventListener("timeupdate", () => {

        let a = parseInt((this.player.currentTime / this.player.duration) * 100);
        myRange.value = a;

        var dur = this.player.duration;
        var cur = this.player.currentTime;

        var Cmin = Math.floor(cur / 60);
        var Csec = Math.floor(cur % 60);
        var min = Math.floor(dur / 60);
        var sec = Math.floor(dur % 60);

        if (min < 10) {
          min = "0" + min;
        }
        if (Cmin < 10) {
          Cmin = "0" + Cmin;
        }
        if (Csec < 10) {
          Csec = "0" + Csec;
        }

        this.timeOfSong = `${min}:${sec}`;
        start.innerText = `${Cmin}:${Csec}`;
      });


      myRange.addEventListener("change", () => {
        this.player.currentTime = myRange.value * this.player.duration / 100;
      });

      this.isActive = false;
    },
    pause() {
      this.player.pause();
      this.isActive = true;
    },
    next() {
      this.index++;

      if (this.index > this.songs.length - 1) {
        this.index = 0;
      };

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;

      if (this.index < 0) {
        this.index = this.songs.length - 1;
      };

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  }
}
</script>