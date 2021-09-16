<template>
  <b-container fluid class="text-left">
    <b-row>
    <b-col class="text-center shadow py-3">
    <h4 v-if="this.returnedTracklist.onNow.song">Now playing:</h4>
    <p v-if="this.returnedTracklist.onNow.song">{{ this.returnedTracklist.onNow.song.artistName }} - {{ this.returnedTracklist.onNow.song.trackName }}</p>
        <audio
         id="stream"
         controls
         src="https://kut.streamguys1.com/kutx-free">
             Your browser does not support the
              <code>audio</code> element.
       </audio>
    </b-col>
    </b-row>
    <b-row class="border-bottom py-3 bg-light">
      <b-col class="font-weight-bold">Play Time</b-col>
      <b-col class="font-weight-bold">Artist</b-col>
      <b-col class="font-weight-bold">Title</b-col>
    </b-row>

    <b-row class="py-3 border shadow" v-if="this.returnedTracklist.onNow.song">
      <b-col>{{ this.returnedTracklist.onNow.song._start_time }}</b-col>
      <b-col><a :href="'https://www.google.com/search?q=' + this.returnedTracklist.onNow.song.artistName" target="_blank">{{ this.returnedTracklist.onNow.song.artistName }}</a></b-col>
      <b-col><a :href="'https://www.youtube.com/results?search_query=' + this.returnedTracklist.onNow.song.artistName + ' ' + this.returnedTracklist.onNow.song.trackName " target="_blank">{{ this.returnedTracklist.onNow.song.trackName }}</a></b-col>

    </b-row>

    <b-row
      v-for="(result, idx) in returnedTracklist.tracklist.results" :key="idx" class="py-3 border">
      <b-col>{{ result.song._start_time }}</b-col>
      <b-col><a :href="'https://www.google.com/search?q=' + result.song.artistName " target="_blank">{{ result.song.artistName }}</a></b-col>
      <b-col><a :href="'https://www.youtube.com/results?search_query=' + result.song.artistName + ' ' + result.song.trackName " target="_blank">{{ result.song.trackName }}</a></b-col>

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
  this.updateTracks();
},
methods: {
  getAudioElement:function(){
    let audioElement = Document.getElementById('stream');
    console.log("audioElement");
    audioElement.play();
  },
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
    },
    updateTracks: function (){
      window.setInterval(()=>{
        this.getTracks();
      }, 15000);
    }
  }
};
</script>

<style>
</style>