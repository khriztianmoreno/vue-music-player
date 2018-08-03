<template>
  <v-card height="330">
    <v-list>
      <v-list-tile
        v-for="(track, index) in playlist"
        :key="track.title"
        v-show="track.display">
        <v-list-tile-content>
          <v-list-tile-title>{{ index }} {{ track.artist }} - {{ track.title }}</v-list-tile-title>
        </v-list-tile-content>
        <v-spacer></v-spacer>
        {{ track.howl.duration() }}
      </v-list-tile>
    </v-list>
  </v-card>
</template>
 
<script>
  export default {
    props: {
      playlist: Array
    }
  }
</script>