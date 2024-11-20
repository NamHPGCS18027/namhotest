<template>
  <div class="music-player">
    <h2>{{ title }}</h2>
    <!-- Đặt YouTube Player vào đây -->
    <div class="containervideo">
      <div id="youtube-player"></div>
    </div>
    <!-- Các điều khiển âm thanh -->
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
// Đảm bảo khai báo YT là đối tượng toàn cục trong mã
/* global YT */

export default {
  name: "MusicPlayer",
  data() {
    return {
      player: null,
      isPlaying: false,
      volume: 0.2,
      title: "Nhạc tự động từ YouTube",
      videoId: "vWav0gngrIE", // Thay thế bằng ID video YouTube của bạn
    };
  },
  methods: {
    initializePlayer() {
      if (typeof YT !== "undefined" && YT.Player) {
        this.player = new YT.Player("youtube-player", {
          videoId: this.videoId,
          playerVars: {
            autoplay: 1, // Tự động phát video khi tải
            controls: 0, // Ẩn các điều khiển của video
            showinfo: 0,
            modestbranding: 1,
            iv_load_policy: 3, // Ẩn thẻ video thông tin
          },
          events: {
            onReady: this.onPlayerReady,
          },
        });
      }
    },
    onPlayerReady(event) {
      event.target.playVideo(); // Phát nhạc tự động khi video sẵn sàng
    },
    togglePlay() {
      if (this.isPlaying) {
        this.player.pauseVideo();
      } else {
        this.player.playVideo();
      }
      this.isPlaying = !this.isPlaying;
    },
    changeVolume() {
      if (this.player) {
        this.player.setVolume(this.volume * 100); // Chuyển volume sang % để điều khiển
      }
    },
  },
  mounted() {
    // Kiểm tra xem script YouTube API đã được tải chưa
    if (typeof YT === "undefined" || typeof YT.Player === "undefined") {
      // Thêm script YouTube API vào trang nếu chưa có
      const script = document.createElement("script");
      script.src = "https://www.youtube.com/iframe_api";
      script.onload = () => {
        // Sau khi script tải xong, khởi tạo player
        this.initializePlayer();
      };
      document.body.appendChild(script);
    } else {
      // Nếu YouTube API đã tải trước, trực tiếp khởi tạo player
      this.initializePlayer();
    }
  },
  beforeDestroy() {
    if (this.player) {
      this.player.destroy(); // Dọn dẹp player khi component bị hủy
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
  visibility: hidden;
  /* Ẩn video nhưng vẫn phát nhạc */
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
