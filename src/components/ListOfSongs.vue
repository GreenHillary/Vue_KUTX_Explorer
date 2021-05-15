<template>
  <b-container fluid class="text-left">
    <b-row>
    <b-col class="text-center shadow py-3">
    <h4 v-if="this.returnedTracklist.onNow.song">Now playing:</h4>
    <p v-if="this.returnedTracklist.onNow.song">{{ this.returnedTracklist.onNow.song.artistName }} - {{ this.returnedTracklist.onNow.song.trackName }}</p>
      <div class="container"> 
        <audio
         controls
         src="https://kut.streamguys1.com/kutx-free">
             Your browser does not support the
              <code>audio</code> element.
       </audio>
      </div>
    </b-col>
    </b-row>
    <b-row class="border-bottom py-3 bg-light">
      <b-col class="font-weight-bold">Play Time</b-col>
      <b-col class="font-weight-bold">Artist</b-col>
      <b-col class="font-weight-bold">Title</b-col>
    </b-row>

    <b-row class="py-3 border shadow" v-if="this.returnedTracklist.onNow.song">
      <b-col>{{ this.returnedTracklist.onNow.song._start_time }}</b-col>
      <b-col>{{ this.returnedTracklist.onNow.song.artistName }}</b-col>
      <b-col>{{ this.returnedTracklist.onNow.song.trackName }}</b-col>

    </b-row>

    <b-row
      v-for="(result, idx) in returnedTracklist.tracklist.results" :key="idx" class="py-3 border">
      <b-col>{{ result.song._start_time }}</b-col>
      <b-col>{{ result.song.artistName }}</b-col>
      <b-col>{{ result.song.trackName }}</b-col>

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