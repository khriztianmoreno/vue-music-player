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
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
  import PlayerTitleBar from './components/PlayerTitleBar.vue'
  import PlayerPlaylistPanel from './components/PlayerPlaylistPanel.vue'

  export default {
    components: {
      PlayerTitleBar,
      PlayerPlaylistPanel
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
        selectedTrack: null
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
    methods: {
      selectTrack (track) {
        this.selectedTrack = track
      }
    }
  }
</script>
