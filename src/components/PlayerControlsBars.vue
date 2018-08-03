<template>
  <div>
    <v-toolbar flat height=90>
      <v-btn flat icon @click="toggleMute">
        <template v-if="!this.muted">
          <v-icon v-if="this.volume >= 0.5">volume_up</v-icon>
          <v-icon v-else-if="this.volume > 0">volume_down</v-icon>
          <v-icon v-else>volume_mute</v-icon>
        </template>
        <v-icon v-show="this.muted">volume_off</v-icon>
      </v-btn>
      <v-slider v-model="volume" @input="updateVolume(volume)" max="1" step="0.1"></v-slider>
      {{this.volume * 100 + '%'}}
      <v-spacer></v-spacer>
      <v-btn outline fab small color="light-blue" @click="skipTrack('prev')">
        <v-icon>skip_previous</v-icon>
      </v-btn>
      <v-btn outline fab small color="light-blue" @click="stopTrack">
        <v-icon>stop</v-icon>
      </v-btn>
      <v-btn outline fab color="light-blue" @click="playTrack()">
        <v-icon large>play_arrow</v-icon>
      </v-btn>
      <v-btn outline fab small color="light-blue" @click="pauseTrack">
        <v-icon>pause</v-icon>
      </v-btn>
      <v-btn outline fab small color="light-blue" @click="skipTrack('next')">
        <v-icon>skip_next</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
    </v-toolbar>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        volume: 0.5,
        muted: false
      }
    },
    created: function () {
      Howler.volume(this.volume)
    },
    methods: {
      playTrack(index) {
        this.$emit('playtrack', index)
      },
      pauseTrack() {
        this.$emit('pausetrack')
      },
      stopTrack() {
        this.$emit('stoptrack')
      },
      skipTrack (direction) {
        this.$emit('skiptrack', direction)
      },
      updateVolume (volume) {
        Howler.volume(volume)
      },
      toggleMute () {
        Howler.mute(!this.muted)
        this.muted = !this.muted
      }
    }
  }
</script>
