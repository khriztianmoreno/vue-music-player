<template>
  <v-app dark>
    <v-content>
      <v-container>
        <player-title-bar />
        <player-controls-bars
          :loop="loop"
          :shuffle="shuffle"
          @playtrack="play"
          @pausetrack="pause"
          @stoptrack="stop"
          @skiptrack="skip"
          @toggleloop="toggleLoop"
          @toggleshuffle="toggleShuffle"
        />
        <player-playlist-panel
          :playlist="playlist"
          :selectedTrack="selectedTrack"
          @selecttrack="selectTrack"
        />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  import PlayerTitleBar from './components/PlayerTitleBar.vue'
  import PlayerPlaylistPanel from './components/PlayerPlaylistPanel.vue'
  import PlayerControlsBars from './components/PlayerControlsBars.vue'

  export default {
    components: {
      PlayerTitleBar,
      PlayerPlaylistPanel,
      PlayerControlsBars
    },
    data () {
      return {
        playlist: [
          {title: "Streets of Sant'Ivo", artist: "Ask Again", howl: null, display: true},
          {title: "Remember the Way", artist: "Ask Again", howl: null, display: true},
          {title: "Guardians", artist: "Ask Again", howl: null, display: true},
          {title: "Crusade of The Castellan", artist: "Ask Again", howl: null, display: true},
          {title: "Land of a Folk Divided", artist: "Ask Again", howl: null, display: true},
          {title: "An Innocent Sword", artist: "Ask Again", howl: null, display: true}
        ],
        selectedTrack: null,
        index: 0,
        playing: false,
        loop: false,
        shuffle: false
      }
    },
    created: function () {
      this.playlist.forEach( (track) => {
        let file = track.title.replace(/\s/g, "_")
        track.howl = new Howl({
          src: [`./playlist/${file}.mp3`],
          onend: () => {
            if (this.loop) {
              this.play(this.index)
            } else {
              this.skip('next')
            }
          }
        })
      })
    },
    computed: {
      currentTrack () {
        return this.playlist[this.index]
      }
    },
    methods: {
      selectTrack (track) {
        this.selectedTrack = track
      },
      play (index) {
        const selectedTrackIndex = this.playlist.findIndex(track => track === this.selectedTrack)

        if (typeof index === 'number') {
          index = index
        } else if (this.selectedTrack) {
          if (this.selectedTrack != this.currentTrack) {
            this.stop()
          }
          index = selectedTrackIndex
        } else {
          index = this.index
        }

        const track = this.playlist[index].howl

        if (track.playing()) {
          return
        } else {
          track.play()
        }

        this.selectedTrack = this.playlist[index]
        this.playing = true
        this.index = index
      },
      pause () {
        this.currentTrack.howl.pause()
        this.playing = false
      },
      stop () {
        this.currentTrack.howl.stop()
        this.playing = false
      },
      skip (direction) {
        let index = 0
        const lastIndex = this.playlist.length - 1

        if (this.shuffle) {
          index = Math.round(Math.random() * lastIndex)
          while (index === this.index) {
            index = Math.round(Math.random() * lastIndex)
          }
        } else if (direction === "next") {
          index = this.index + 1
          if (index >= this.playlist.length) {
            index = 0
          }
        } else {
          index = this.index - 1
          if (index < 0) {
            index = this.playlist.length - 1
          }
        }

        this.skipTo(index)
      },
      skipTo (index) {
        if (this.currentTrack) {
          this.currentTrack.howl.stop()
        }

        this.play(index)
      },
      toggleLoop (value) {
        this.loop = value
      },
      toggleShuffle (value) {
        this.shuffle = value
      }
    }
  }
</script>
