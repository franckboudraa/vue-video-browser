<template>
  <div class="container">
    <div style="text-align:center" class="mt-3"><h1>Vue Video Browser</h1></div>
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail :video="selectedVideo"></VideoDetail>
      <VideoList :videos="videos" @videoSelect="onVideoSelect"></VideoList>
    </div>
    <div style="text-align:center">
      <small
        >Vue Video Browser - made to learn VueJS -
        <a
          href="https://github.com/franckboudraa/vue-video-browser"
          target="_blank"
          >github source</a
        ></small
      >
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar";
import VideoDetail from "./components/VideoDetail";
import VideoList from "./components/VideoList";
const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY;

export default {
  name: "App",
  components: {
    SearchBar,
    VideoDetail,
    VideoList
  },
  data() {
    return { selectedVideo: null, videos: [] };
  },
  methods: {
    onTermChange(searchTerm) {
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            key: API_KEY,
            type: "video",
            part: "snippet",
            q: searchTerm
          }
        })
        .then(response => {
          this.videos = response.data.items;
        });
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    }
  }
};
</script>
