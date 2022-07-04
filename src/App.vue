<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col1 col-md-6 text-center pb-2">
        <h1 class="category text-light">
          Mobile Suit Gundam{{ current.category }}
        </h1>
        <img class="avatar text-center my-2" v-bind:src="current.cover" />
        <h3 class="title accent">{{ current.title }}</h3>
        <span class="artist text-secondary">{{ current.artist }}</span>
      </div>
      <div class="col2 col-md-6" style="overflow-y: auto; height: 80vh">
        <div class="playlist d-grid gap-2">
          <div
            class="
              song
              p-1
              pe-3
              d-flex
              justify-content-between
              align-items-center
            "
            v-for="(song, index) in songs"
            :key="song.src"
            @click="play(song, index)"
            :class="song.src == current.src ? 'song-playing' : 'song'"
          >
            <span class="d-flex align-items-center">
              <img
                v-if="isPlaying && songindex == index"
                src="https://www.hot21radio.com/img/equalizer.gif"
                style="width: 2em; height: 2em"
              />
              <span v-else>
                <div
                  class="d-flex align-items-center justify-content-center"
                  style="width: 2em; height: 2em"
                >
                  <span> {{ index + 1 }}</span>
                </div>
              </span>
              <img
                class="mini-avatar me-2"
                v-bind:src="song.cover"
                style="
                  width: 2rem;
                  height: 2rem;
                  object-fit: cover;
                  border-radius: 10%;
                "
              />
              <span>{{ song.title }}</span>
            </span>
            <span class="cat-p text-end">
              Mobile Suit Gundam{{ song.category }}
            </span>
            <span class="cat-m text-end"> MSG{{ song.category }} </span>
          </div>
        </div>
      </div>
    </div>

    <div class="footer fixed-bottom p-1">
      <div class="controls text-center m-1">
        <div>
          <!-- repeat -->
          <i
            class="material-icons mx-3 text-secondary"
            v-if="repeatStat == 0"
            @click="repeat"
            >repeat</i
          >
          <i
            class="material-icons mx-3"
            v-else-if="repeatStat == 1"
            @click="repeat"
            >repeat</i
          >
          <i class="material-icons mx-3" @click="repeat" v-else>repeat_one</i>
          <!-- prev -->
          <i class="material-icons m-1" @click="prev">skip_previous</i>
          <!-- play/pause -->
          <i class="material-icons m-2 mx-3" v-if="!isPlaying" @click="play"
            >play_circle_filled</i
          >
          <i class="material-icons m-2 mx-3" v-else @click="pause"
            >pause_circle_filled</i
          >
          <!-- next -->
          <i class="material-icons m-1" @click="next">skip_next</i>
          <!-- shuffle -->
          <i
            class="material-icons mx-3 text-secondary"
            v-if="!isShuffle"
            @click="shuffle"
            >shuffle</i
          >
          <i class="material-icons mx-3" v-else @click="shuffle">shuffle</i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      player: new Audio(),
      isPlaying: false,
      isShuffle: false,
      repeatStat: 0,
      songindex: 0,
      current: {},
      songs: [
        {
          title: "Into The Sky",
          artist: "SawanoHiroyuki[nZk]Tielle",
          category: " Unicorn",
          src: require("./assets/music/Unicorn/Into The Sky - SawanoHiroyuki[nZk]Tielle.mp3"),
          cover:
            "https://cdn.donmai.us/original/da/54/da543079ee3285fa9c1e5d918fad1a50.jpg",
        },
        {
          title: "Ash Like Snow",
          artist: "The Brilliant Green",
          category: " 00",
          src: require("./assets/music/00/Ash Like Snow - The Brilliant Green.mp3"),
          cover:
            "https://c4.wallpaperflare.com/wallpaper/448/77/82/mobile-suit-gundam-00-gundam-setsuna-f-seiei-gundam-00-exia-wallpaper-preview.jpg",
        },
        {
          title: "DAYBREAK'S BELL",
          artist: "L'Arc-en-Ciel",
          category: " 00",
          src: require("./assets/music/00/DAYBREAK'S BELL - L'Arc-en-Ciel.mp3"),
          cover:
            "https://konachan.com/sample/86fe1ec3ae89504ebd5661b08f3e15ab/Konachan.com%20-%2041454%20sample.jpg",
        },
        {
          title: "Hakanaku mo Towa no Kanashi",
          artist: "UVERworld",
          category: " 00",
          src: require("./assets/music/00/Hakanaku mo Towa no Kanashi - UVERworld.mp3"),
          cover:
            "https://w0.peakpx.com/wallpaper/353/221/HD-wallpaper-the-evolution-setsuna-particle-gundam-00-raiser-gundam-gundam-exia-gundam-green-mecha-gn-anime-00-raiser-00-gundam.jpg",
        },
        {
          title: "Namida no Mukou",
          artist: "Stereopony",
          category: " 00",
          src: require("./assets/music/00/Namida no Mukou - Stereopony.mp3"),
          cover: 
            "https://i.pinimg.com/originals/c9/06/c4/c906c47737c71ccba6acebb2d5189808.jpg",
        },
        {
          title: "Trust you",
          artist: "Yuna Ito",
          category: " 00",
          src: require("./assets/music/00/Trust you - Yuna Ito.mp3"),
          cover:
            "https://static.zerochan.net/Mobile.Suit.Gundam.00.full.315250.jpg",
        },
        {
          title: "Fighter",
          artist: "KANA-BOON",
          category: " IBO",
          src: require("./assets/music/ibo/Fighter - KANA-BOON.mp3"),
          cover: "https://static.zerochan.net/Kidou.Senshi.Gundam%3A.Tekketsu.no.Orphans.full.2964699.jpg",
        },
        {
          title: "Freesia",
          artist: "Uru",
          category: " IBO",
          src: require("./assets/music/ibo/Freesia - Uru.mp3"),
          cover: "https://i.pinimg.com/originals/5a/f9/a9/5af9a99c3d56c37913e36a557a6a4600.jpg",
        },
        {
          title: "Rage of Dust",
          artist: "SPYAIR",
          category: " IBO",
          src: require("./assets/music/ibo/Rage of Dust - SPYAIR.mp3"),
          cover: "https://1.bp.blogspot.com/-c3SgVNDAYPs/WAQxOp5CFsI/AAAAAAALI-U/Eobpx4i3-PI75xBNHMXioiqrcpPZBCRJgCLcB/s1600/Cu4ElB9UIAETBnc.jpg",
        },
        {
          title: "Raise Your Flag",
          artist: "MAN WITH A MISSION",
          category: " IBO",
          src: require("./assets/music/ibo/Raise Your Flag - MAN WITH A MISSION.mp3"),
          cover: "https://wallpaperaccess.com/full/2239563.jpg",
        },
        {
          title: "Shōnen no Hate",
          artist: "GRANRODEO",
          category: " IBO",
          src: require("./assets/music/ibo/Shōnen no Hate - GRANRODEO.mp3"),
          cover: "https://i.pinimg.com/originals/68/fd/64/68fd641e143e87a5bf9f999d315b9f68.jpg",
        },
        {
          title: "STEEL - Tetsuketsu no Kizuna",
          artist: "TRUE",
          category: " IBO",
          src: require("./assets/music/ibo/STEEL -Tetsuketsu no Kizuna - TRUE.mp3"),
          cover: "https://i.pinimg.com/originals/a9/03/53/a90353a8f923d0bb258cbee5dc3ba48f.jpg",
        },
        {
          title: "Survivor",
          artist: "BLUE ENCOUNT",
          category: " IBO",
          src: require("./assets/music/ibo/Survivor - BLUE ENCOUNT.mp3"),
          cover: "https://preview.redd.it/13hkh1qjy9b81.png?auto=webp&s=90bfa5f2f9b0e073210d781492298315070cdcd9",
        },
        {
          title: "Tears of Orphan",
          artist: "MISIA",
          category: " IBO",
          src: require("./assets/music/ibo/Tears of Orphan - MISIA.mp3"), 
          cover: "https://i.pinimg.com/originals/7d/7b/d2/7d7bd2ee683b2204bb4bc38545c20ca4.jpg",
        },
        {
          title: "Akatsuki no Kuruma",
          artist: "FictionJunction Yuuka",
          category: " SEED",
          src: require("./assets/music/seed/Akatsuki no Kuruma - FictionJunction Yuuka.mp3"),
          cover: 
            "https://konachan.com/image/ab32f684f0a4709f6d1efc79e3771900/Konachan.com%20-%2015287%20gundam_seed%20kira_yamato%20lacus_clyne%20mobile_suit_gundam.jpg",
        },
        {
          title: "Anna ni Issho Datta no ni",
          artist: "See-Saw",
          category: " SEED",
          src: require("./assets/music/seed/Anna ni Issho Datta no ni - See-Saw.mp3"),
          cover:
            "https://static.zerochan.net/Mobile.Suit.Gundam.SEED.full.18713.jpg",
        },
        {
          title: "Believe",
          artist: "Nami Tamaki",
          category: " SEED",
          src: require("./assets/music/seed/Believe - Nami Tamaki.mp3"),
          cover:
            "https://safebooru.org//images/3361/3ac8909e841a83e4197104ccd7b6d16e.jpeg",
        },
        {
          title: "Distance",
          artist: "FictionJunction",
          category: " SEED",
          src: require("./assets/music/seed/Distance - FictionJunction.mp3"),
          cover:
            "https://c4.wallpaperflare.com/wallpaper/283/703/809/gundam-seed-shinn-shinn-and-stellar-anime-gundam-seed-hd-art-wallpaper-preview.jpg",
        },
        {
          title: "Ignited",
          artist: "T.M.Revolution",
          category: " SEED",
          src: require("./assets/music/seed/Ignited - T.M.Revolution.mp3"),
          cover:
            "https://static.zerochan.net/Impulse.Gundam.full.3093381.jpg",
        },
        {
          title: "Invoke",
          artist: "T.M.Revolution",
          category: " SEED",
          src: require("./assets/music/seed/Invoke - T.M.Revolution.mp3"),
          cover:
            "https://media.karousell.com/media/photos/products/2021/10/16/metal_build_freedom_c2_justice_1634393854_9a5bb093_progressive.jpg",
        },
        {
          title: "Kimi wa Boku ni Niteiru",
          artist: "See-Saw",
          category: " SEED",
          src: require("./assets/music/seed/Kimi wa Boku ni Niteiru - See-Saw.mp3"),
          cover:
            "https://www.wallpapertip.com/wmimgs/197-1972487_sunrise-mobile-suit-gundam-seed-destiny-haro-cagalli.jpg",
        },
        {
          title: "Life Goes On",
          artist: "Mika Arisaka",
          category: " SEED",
          src: require("./assets/music/seed/Life Goes On - Mika Arisaka.mp3"),
          cover:
            "https://images6.alphacoders.com/760/760182.jpg",
        },
        {
          title: "Meteor",
          artist: "T.M.Revolution",
          category: " SEED",
          src: require("./assets/music/seed/Meteor - T.M.Revolution.mp3"),
          cover:
            "https://static.zerochan.net/Strike.Gundam.full.3412053.jpg",
        },
        {
          title: "Realize",
          artist: "Nami Tamaki",
          category: " SEED",
          src: require("./assets/music/seed/Realize - Nami Tamaki.mp3"),
          cover:
            "https://static.zerochan.net/Destiny.Gundam.full.3440475.jpg",
        },
        {
          title: "Reason",
          artist: "Nami Tamaki",
          category: " SEED",
          src: require("./assets/music/seed/Reason - Nami Tamaki.mp3"),
          cover:
            "https://i.pinimg.com/originals/ae/49/ff/ae49ff8aa0824ed76fdf23f539be3696.png",
        },
        {
          title: "RIVER",
          artist: "Tatsuya Ishii",
          category: " SEED",
          src: require("./assets/music/seed/RIVER - Tatsuya Ishii.mp3"),
          cover:
            "https://ghostlightning.files.wordpress.com/2010/12/image14.png",
        },

        {
          title: "Zips",
          artist: "T.M.Revolution",
          category: " SEED",
          src: require("./assets/music/seed/Zips - T.M.Revolution.mp3"),
          cover:
            "https://i.pinimg.com/originals/8c/98/5b/8c985b77a4df8cfb9fc7a681d377d57e.jpg",
        },
      ],
    };
  },
  methods: {
    play(song, index) {
      // if (this.songindex == index) return;
      if (typeof index != "undefined") {
        this.songindex = index;
      }
      if (typeof song.src != "undefined" && index != null) {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      if (this.isShuffle) {
        this.songindex = Math.floor(Math.random() * this.songs.length);
      } else {
        this.songindex++;
      }

      if (this.songindex > this.songs.length - 1) {
        this.songindex = 0;
      }
      this.current = this.songs[this.songindex];
      this.play(this.current, this.songindex);
    },
    prev() {
      if (this.isShuffle) {
        this.songindex = Math.floor(Math.random() * this.songs.length);
      } else {
        this.songindex--;
      }

      if (this.songindex < 0) {
        this.songindex = this.songs.length - 1;
      }
      this.current = this.songs[this.songindex];
      this.play(this.current, this.songindex);
    },
    shuffle() {
      this.isShuffle = !this.isShuffle;
    },
    repeat() {
      if (this.repeatStat < 2) {
        this.repeatStat++;
      } else {
        this.repeatStat = 0;
      }
    },
  },
  created() {
    this.current = this.songs[this.songindex];
    this.player.src = this.current.src;

    this.player.onended = () => {
      if (this.repeatStat == 0) {
        if (this.songindex == this.songs.length - 1) {
          this.player.pause();
          this.isPlaying = false;
          this.current = this.songs[0];
          return;
        }
      }
      if (this.repeatStat == 2) {
        this.play(this.current, this.songindex);
      } else {
        this.next();
      }
    };
  },
};
</script>

<style>
body {
  background-color: black;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.footer {
  background-color: #181818;
}
.avatar {
  width: 24rem;
  height: 24rem;
  border-radius: 5%;
  object-fit: cover;
}

.accent {
  color: #ff6600;
}
.song {
  cursor: pointer;
  border-radius: 5px;
  color: white;
}
.song-playing {
  color: #ff6600;
}
.song:hover {
  background-color: rgba(196, 193, 193, 0.171);
}
.cat-p {
  display: block;
}
.cat-m {
  display: none;
}
.col2 {
  padding-left: 3rem;
  padding-right: 3rem;
  padding-bottom: 3rem;
}
.container-fluid {
  padding-top: 3rem;
}
.material-icons {
  color: #ff6600;
  cursor: pointer;
}

@media screen and (max-width: 900px) {
  .avatar {
    width: 15rem;
    height: 15rem;
  }
  .cat-p {
    display: none;
  }
  .cat-m {
    display: block;
  }
  .col1 {
    margin-bottom: 0.5em;
  }
  .col2 {
    padding-left: 0.5em;
    padding-right: 0.5em;
    max-height: 42vh;
  }
  .container-fluid {
    padding-top: 1rem;
  }
}
</style>
