<template>
  <b-container fluid class="text-left">
    <b-row class="border-bottom shadow py-1">
      <b-col>Play Time</b-col>
      <b-col>Artist</b-col>
      <b-col>Title</b-col>
      <b-col>Connections</b-col>
      <b-col>Releases</b-col>
    </b-row>

    <b-row class="py-3 border border-success" v-if="this.returnedTracklist.onNow.song">
      <b-col>{{ this.returnedTracklist.onNow.song._start_time }}</b-col>
      <b-col>{{ this.returnedTracklist.onNow.song.artistName }}</b-col>
      <b-col>{{ this.returnedTracklist.onNow.song.trackName }}</b-col>
      <b-col
        ><a href=""
          ><img class="img-fluid" src="./../assets/whosampled.png" /></a
      ></b-col>
      <b-col
        ><a href=""><img class="img-fluid" src="./../assets/discogs.png" /></a
      ></b-col>
    </b-row>

    <b-row
      v-for="(result, idx) in returnedTracklist.tracklist.results" :key="idx" class="py-3 border">
      <b-col>{{ result.song._start_time }}</b-col>
      <b-col>{{ result.song.artistName }}</b-col>
      <b-col>{{ result.song.trackName }}</b-col>
      <b-col
        ><a href=""
          ><img class="img-fluid" src="./../assets/whosampled.png" /></a
      ></b-col>
      <b-col
        ><a href=""><img class="img-fluid" src="./../assets/discogs.png" /></a
      ></b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data: function () {
    return {
      returnedTracklist: {
        onNow: {
          song: {
            _start_time: '',
            artistName: '',
            trackName: ''
          }
        },
        tracklist:{
          result: {
            song: {
              _start_time: '',
              artistName: '',
              trackName: ''
            
          }
        }
      }
    }
  };
},
mounted: function(){
  this.getTracks();
},
methods: {
  getTracks:function(){
    fetch("https://api.composer.nprstations.org/v1/widget/50ef24ebe1c8a1369593d032/tracks?format=json&limit=20")
      .then(async response => {
        const data = await response.json(); 

        if (!response.ok) {
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }

        this.returnedTracklist = data;
      })
      .catch(error => {
        this.errorMessage = error;
        console.error("There was an error!", error);
      });
    }
  }
};
</script>

<style>
</style>