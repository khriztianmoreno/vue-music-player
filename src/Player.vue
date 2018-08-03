<template>
  <v-app dark>
    <v-content>
      <v-container>
        <player-title-bar />
        <player-playlist-panel
          :playlist="playlist"
          :selectedTrack="selectedTrack"
          @selecttrack="selectTrack"
        />
        <player-controls-bars
          @playtrack="play"
          @pausetrack="pause"
          @stoptrack="stop"
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
        playing: false
      }
    },
    created: function () {
      this.playlist.forEach( (track) => {
        let file = track.title.replace(/\s/g, "_")
        track.howl = new Howl({
          src: [`./playlist/${file}.mp3`]
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
      }
    }
  }
</script>
