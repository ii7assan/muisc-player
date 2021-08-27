<template>
  <div>
    <v-container fluid class="text-capitalize">
      <v-row>
        <v-col jusitfy="center" align="center" class="pa-5 ma-3">
          <section class="text-center display-1">
            <h4>{{ current.title }} - {{ current.artist }}</h4>
          </section>
            <Controls @play="play" @pause="pause" @next="next" @prev="prev" :isPlaying="isPlaying"/>
        </v-col>
        <v-col cols="12" jusitfy="center" align="center">
          <Playlist :songs="songs" :current="current" @play="play" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import Controls from './controls.vue';
import Playlist from './playlist.vue';
export default {
  components: { Playlist,Controls },
  data() {
    return {
      isPlaying: false,
      songs: [
        {
          title: "Invincible",
          artist: "Deaf Kev",
          src: "https://raw.githubusercontent.com/muhammederdem/mini-player/master/mp3/9.mp3",
        },
        {
          title: "overdose",
          artist: "grandson",
          src: "https://raw.githubusercontent.com/muhammederdem/mini-player/master/mp3/8.mp3",
        },
        {
          title: "comeback kid",
          artist: "lindi ortega",
          src: "https://raw.githubusercontent.com/muhammederdem/mini-player/master/mp3/7.mp3",
        },
      ],
      index: 0,
      current: {},
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
      this.player.addEventListener("ended", () => {
        this.index++;
        if (this.index > this.current.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
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
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>

</style>