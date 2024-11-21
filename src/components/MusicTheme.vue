<template>
  <div class="music-player">
    <h2>{{ title }}</h2>
    <div class="containervideo">
      <div id="youtube-player"></div>
    </div>
    <div class="controls">
      <button @click="togglePlay">
        {{ isPlaying ? "Dừng" : "Phát" }}
      </button>
      <div class="volume-control">
        <label for="volume">Âm lượng:</label>
        <input id="volume" type="range" min="0" max="1" step="0.1" v-model="volume" @input="changeVolume" />
      </div>
    </div>
    <p v-if="isPlaying">🎵 Đang phát...</p>
    <p v-else>⏸ Đã dừng.</p>
  </div>
</template>

<script>
// Global object YT should be defined
/* global YT */

export default {
  name: "MusicPlayer",
  data() {
    return {
      player: null,
      isPlaying: false,
      volume: 0.2,
      title: "Nhạc tự động từ YouTube",
      videoId: "vWav0gngrIE", // Your video ID here
    };
  },
  methods: {
    initializePlayer() {
      if (typeof YT !== "undefined" && YT.Player) {
        this.player = new YT.Player("youtube-player", {
          videoId: this.videoId,
          playerVars: {
            autoplay: 1,
            controls: 0,
            showinfo: 0,
            modestbranding: 1,
            iv_load_policy: 3,
          },
          events: {
            onReady: this.onPlayerReady,
            onStateChange: this.onPlayerStateChange,
            onError: this.onPlayerError,
          },
        });
      } else {
        console.error("YouTube API not loaded or YT.Player is undefined");
      }
    },
    onPlayerReady(event) {
      console.log("YouTube Player is ready");
      event.target.playVideo();
    },
    onPlayerStateChange(event) {
      if (event.data === YT.PlayerState.PLAYING) {
        this.isPlaying = true;
      } else {
        this.isPlaying = false;
      }
    },
    onPlayerError(event) {
      console.error("YouTube Player Error: ", event.data);
    },
    togglePlay() {
      if (this.player) {
        if (this.isPlaying) {
          this.player.pauseVideo();
        } else {
          this.player.playVideo();
        }
        this.isPlaying = !this.isPlaying;
      }
    },
    changeVolume() {
      if (this.player) {
        this.player.setVolume(this.volume * 100); // Convert volume to percentage
      }
    },
  },
  mounted() {
    // Ensure the YouTube API script is loaded
    if (typeof YT === "undefined" || typeof YT.Player === "undefined") {
      // Add the YouTube API script if it's not already loaded
      const script = document.createElement("script");
      script.src = "https://www.youtube.com/iframe_api";
      
      // This will ensure that the player is initialized once the API is ready
      window.onYouTubeIframeAPIReady = () => {
        this.initializePlayer();
      };
      
      document.body.appendChild(script);
    } else {
      // If YouTube API is already loaded, initialize the player immediately
      this.initializePlayer();
    }
  },
  beforeDestroy() {
    if (this.player) {
      this.player.destroy(); // Clean up player when component is destroyed
    }
  },
};
</script>

<style scoped>
.music-player {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px auto;
  max-width: 300px;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  background: #f9f9f9;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
}

.containervideo {
  visibility: hidden;
  height: 5px;
}

#youtube-player {
  width: 100%;
  height: 200px;
  opacity: 0; /* Invisible but still plays music */
}

.controls {
  margin-top: 20px;
  text-align: center;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.volume-control {
  margin-top: 10px;
}

input[type="range"] {
  width: 100px;
}
</style>
