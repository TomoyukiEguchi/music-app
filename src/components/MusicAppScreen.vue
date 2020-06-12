<template>
  <section class='player'>
    <div class='screen'>
      <div class='phoneScreen' :style="{ backgroundImage: `url(${currentTrack.imgLink})` }"></div>
      <div class='menu'><v-fa icon='chevron-left' /><v-fa icon='ellipsis-v' /></div>
      <transition-group
        name='carousel'
        class='carousel'
        tag="div">
        <div
          v-for="slide in slides" 
          class='slide'
          :key="slide.src">
          <img class='slide__image' :src='slide.imgLink'>
        </div>
      </transition-group>
      <div class='song-details'>
        <h2 class='song-title'>{{ currentTrack.title }}</h2>
        <p class='artist'>{{ currentTrack.artist }}</p>
      </div>
      <div class='progress'>
        <div class='progress__top'>
          <span class='progress__time'>{{ currentTime }}</span><span class='progress__duration'>{{ duration }}</span>
        </div>
        <div class='progress__bar'>
          <div class='progress__current' :style='{ width : barWidth }'></div>
        </div>
      </div>
      <div class='controls'>
        <span class='prev' @click="previous"><v-fa icon='step-backward' /></span>
        <span class='playpause' @click="play">
          <span v-if="isTimerPlaying"><v-fa icon='pause' /></span>
          <span v-else><v-fa icon="play" /></span>
        </span>
        <span class='next' @click="next"><v-fa icon='step-forward' /></span>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'carousel',
  data () {
    return {
      audio: null,
      circleLeft: null,
      barWidth: null,
      duration: null,
      currentTime: null,
      isTimerPlaying: false,
      currentTrack: null,
      currentTrackIndex: 0,
      slides: [
        {
          imgLink: require('../assets/Galaxy_2_Galaxy.jpg'),
          title: 'Hi-Tech Jazz',
          artist: 'Galaxy 2 Galaxy',
          src: require('../assets/songs/Hi-Tech_Jazz.mp3'),
        },
        {
          imgLink: require('../assets/mesa_TrueThoughts.jpg'),
          title: 'True Thoughts',
          artist: 'Anomaly',
          src: require('../assets/songs/True_Thoughts.mp3'),
        },
        {
          imgLink: require('../assets/Random_Access_Memories.jpg'),
          title: "Get Lucky",
          artist: "Daft Punk",
          src: require('../assets/songs/Get_lucky.mp3'),
        },
        {
          imgLink: require('../assets/Rained_Out.jpg'),
          title: "Rained Out",
          artist: "Blktop Project",
          src: require('../assets/songs/Rained_Out.mp3'),
        },
        {
          imgLink: require('../assets/Radiation_Ruling_The_Nation.jpg'),
          title: 'Radiation Ruling the Nation',
          artist: 'Massive Attack',
          src: require('../assets/songs/Radiation_Ruling_the_Nation.mp3'),
        }
      ]
    }
  },
  methods: {
    play () {
      if (this.audio.paused) {
        this.audio.play();
        this.isTimerPlaying = true;
      } else {
        this.audio.pause();
        this.isTimerPlaying = false;
      }
    },
    next () {
      const first = this.slides.shift()
      this.slides = this.slides.concat(first)

      if (this.currentTrackIndex < this.slides.length - 1) {
        this.currentTrackIndex++;
      } else {
        this.currentTrackIndex = 0;
      }

      let centeroflength = ( this.slides.length + 1 ) / 2 - 1;
      this.currentTrack = this.slides[centeroflength];
      this.resetPlayer();
    },
    previous () {
      const last = this.slides.pop()
      this.slides = [last].concat(this.slides)

      if (this.currentTrackIndex > 0) {
        this.currentTrackIndex--;
      } else {
        this.currentTrackIndex = this.slides.length - 1;
      }

      let centeroflength = ( this.slides.length + 1 ) / 2 - 1;
      this.currentTrack = this.slides[centeroflength];
      this.resetPlayer();
    },
    resetPlayer() {
      this.barWidth = 0;
      this.circleLeft = 0;
      this.audio.currentTime = 0;
      this.audio.src = this.currentTrack.src;
      setTimeout(() => {
        if(this.isTimerPlaying) {
          this.audio.play();
        } else {
          this.audio.pause();
        }
      }, 300);
    },
    generateTime() {
      let width = (100 / this.audio.duration) * this.audio.currentTime;
      this.barWidth = width + "%";
      this.circleLeft = width + "%";
      let durmin = Math.floor(this.audio.duration / 60);
      let dursec = Math.floor(this.audio.duration - durmin * 60);
      let curmin = Math.floor(this.audio.currentTime / 60);
      let cursec = Math.floor(this.audio.currentTime - curmin * 60);
      if (durmin < 10) {
        durmin = "0" + durmin;
      }
      if (dursec < 10) {
        dursec = "0" + dursec;
      }
      if (curmin < 10) {
        curmin = "0" + curmin;
      }
      if (cursec < 10) {
        cursec = "0" + cursec;
      }
      this.duration = durmin + ":" + dursec;
      this.currentTime = curmin + ":" + cursec;
    },
  },
  created() {
    let vm = this;
    let centeroflength = ( this.slides.length + 1 ) / 2 - 1;
    this.currentTrack = this.slides[centeroflength];
    this.audio = new Audio();
    this.audio.src = this.currentTrack.src;
    this.audio.ontimeupdate = function() {
      vm.generateTime();
    };
    this.audio.onloadedmetadata = function() {
      vm.generateTime();
    };
    this.audio.onended = function() {
      vm.next();
      this.isTimerPlaying = true;
    };
  }
}
</script>

<style scoped>
.player {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-position: left;
  height: calc(100% + 3px);
}
.phoneScreen {
  background-size: cover;
  background-position: center;
  display: block;
  position: absolute; 
  backdrop-filter: blur(8px);
  filter: blur(20px);
  width: 250px;
  min-height: 510px;
  overflow: hidden;
  mask-image: url('../assets/silhouette.png');
  border-radius: 25px;
}
.screen {
  position: relative;
  width: 250px;
  min-height: 510px;
  background-color: rgb(0,0,0);
  background-color: rgba(0,0,0, 0.5);
  overflow: hidden;
  border-radius: 25px;
}
.menu {
  padding: 21px;
  position: relative;
  z-index: 2;
  font-size: 14px;
  display: flex;
  justify-content: space-between;
  color: #fff;
}
.carousel {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 250px;
  height: 200px;
  overflow: hidden;
}
.slide {
  transform: translateZ(0) scale(1.0, 1.0);
  height: 20em;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  transition: transform 0.3s ease-in-out;
  opacity: 1;
  margin: 1em;
  flex: 0 0 20em;
}
.slide:first-of-type {
  opacity: 0;
}
.slide:last-of-type {
  opacity: 0;
}
.slide__image {
  width: 170px;
  min-height: 170px;
  display: flex;
  box-shadow: 1px 1px 0.75rem black;
}
.song-details {
  position: relative;
  margin-bottom: 35px;
  z-index: 2;
  text-align: center;
  color: #fff;
}
.song-title {
  font-size: 17px;
}
.progress {
  margin: 1.8em;
  position: relative;
  z-index: 2;
  color: #fff;
}
.progress__top {
  font-size: 13px;
  display: flex;
  justify-content: space-between;
}
.progress__bar {
  height: 2px;
  width: 100%;
  z-index: 2;
  background-color: #fff;
  display: inline-block;
  border-radius: 10px;
}
.progress__current {
  height: inherit;
  width: 0%;
  background-color: #afafaf;
  border-radius: 10px;
}
.controls {
  height: 35px;
  margin-top: 35px;
  font-size: 18px;
  position: relative;
  z-index: 2;
  color: #fff;
}
.playpause {
  margin-left: 30px;
  margin-right: 30px;
}
</style>