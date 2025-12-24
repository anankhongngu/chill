<template>
    <div class="w3-row-padding d-flex">
        <div class="banner">
            <h1 class="banner-title">Góc Chữa Lành</h1>
        </div>
        <div class="w3-col-25 w3-container  sticky">
            <div class="w3-margin-bottom">
                <div class="search-bar">
                    <button @click="searchSongs" class="search-icon"><i class="fa fa-search"></i></button>
                    <input v-model="query" @keyup.enter="searchSongs" type="text" placeholder="Tìm kiếm bài hát..." />
                </div> 
            </div>
            <div class="w3-white w3-margin-bottom">
                <div class="player wapper-player">
                    <div class="title">
                        <h4>Now playing:</h4>
                        <h2>{{ currentTitle }}</h2>
                    </div>
                    <div class="cd">
                        <div class="cd-thumb" ref="cdThumb" :style="{ backgroundImage: `url(${ currentBackground })` }"></div>
                        <div class="hidden" id="youtube-player"></div>
                    </div> 
                    <div class="control">
                        <div class="btn btn-repeat"  @click="repeatVideo" :class="{ 'active': isRepeat }">
                            <i class="fa fa-repeat"></i>
                        </div>
                        <div class="btn btn-prev" @click="prevVideo">
                            <i class="fa fa-step-backward"></i>
                        </div>
                        <div class="btn btn-toggle-play" @click="togglePlayPause"> 
                            <i class="fa fa-pause icon-pause" v-if="isPlaying"></i>
                            <i class="fa fa-play icon-play" v-else></i>
                        </div>
                        <div class="btn btn-next" @click="nextVideo">
                            <i class="fa fa-step-forward"></i>
                        </div>
                        <div class="btn btn-random" @click="toggleRandom" :class="{ 'active': isRandom }">
                            <i class="fa fa-random"></i>
                        </div>
                    </div>
                    <div class="progress-bar">
                        <div class="progress" :style="{ width: progressPercentage + '%' }"></div>
                    </div>
                </div>
            </div>   
        </div>
        <div class="w3-col-75 w3-container  ">
            <div class="music-section w3-margin-bottom">       
                <div class="music-suggestions"  v-if="videos.length">
                    <h2 v-if="isSearch">Kết quả tìm kiếm: {{  query }}</h2>
                    <h2 v-else>Gợi Ý Cho Bạn</h2>
                    <div class="songs-list">
                        <div class="song-item" v-for="(video,index) in videos" :key="video.id" 
                        :class="{ 'active': currentVideoId === video.id }"
                        @click="playVideo(video.id,index, video.thumbnail, video.title)">
                            <img :src="video.thumbnail" :alt="video.title">
                            <div class="song-info">
                                <p class="song-title">{{ video.title }}</p>
                                <p class="song-artist">{{ video.channelTitle }}</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
            query: '',
            player: null,
            videos: [],
            currentTitle:'', 
            currentVideoId: null,
            currentBackground: '',
            apiKey: 'AIzaSyCiatuTOF3ptOossuQWyBuKFsi0o2k9YtE',
            selectedVideo: null, 
            regionCode: 'VN',
            videoFirst: {},
            currentVideoIndex: 0,
            isPlaying: false,
            isRandom: false,
            isRepeat: false,
            isSearch: false,
            cdThumbAnimation: null, 
            progressPercentage: 0,
            progressInterval: null
        };
    }, 
    computed: {
        selectedVideoUrl() {
        if (this.selectedVideo) {
            return `https://www.youtube.com/embed/${this.selectedVideo.id.videoId}`;
        }
        return '';
        },
    },
    mounted() {
        
    },
    methods: {
        async searchSongs() {
            if (!this.query) return;
            const url = `https://www.googleapis.com/youtube/v3/search?part=snippet&q=${encodeURIComponent(
                this.query
            )}&type=video&key=${this.apiKey}&maxResults=30`;

            try {
                const response = await axios.get(url);
                this.videos = response.data.items.map(item => ({
                    id: item.id.videoId,
                    title: item.snippet.title,
                    thumbnail: item.snippet.thumbnails.medium.url,
                    channelTitle: item.snippet.channelTitle,
                }));
                this.isSearch = true;

                // if (this.videos.length > 0) {
                //     this.playVideoSafe(0); // Auto play video đầu tiên sau khi search
                // }
            } catch (error) {
                console.error('Lỗi khi gọi API:', error);
            }
        },
        async fetchTrendingMusic() {
            const url = `https://www.googleapis.com/youtube/v3/videos?part=snippet&chart=mostPopular&regionCode=${this.regionCode}&videoCategoryId=10&maxResults=30&key=${this.apiKey}`;

            try {
                const response = await axios.get(url)
                this.videos = response.data.items.map(item => ({
                    id: item.id,
                    title: item.snippet.title,
                    thumbnail: item.snippet.thumbnails.medium.url,
                    channelTitle: item.snippet.channelTitle,
                }))
                
                if (this.videos.length > 0) {
                    this.videoFirst = {
                        idFirst: this.videos[0].id,
                        titleFirst: this.videos[0].title,
                        thumbnailFirst: this.videos[0].thumbnail,
                        channelTitleFirst: this.videos[0].channelTitle,
                    }
                }
                this.initPlayer(this.videoFirst.idFirst,0)
            } catch (error) {
                console.error('Lỗi khi lấy dữ liệu:', error)
            }
        }, 
        initPlayerWrapper() {
            if (this.videos.length > 0) {
                this.initPlayer(this.videos[0].id, 0);
            }
        },
        playVideo(videoId,index,thumbnail,title) {
            this.currentVideoIndex = index
            this.currentVideoId = videoId
            this.currentBackground = thumbnail
            this.currentTitle = title

            if (this.player && typeof this.player.loadVideoById === 'function') {
                this.player.loadVideoById(videoId)
            } else {
                this.initPlayer(videoId, index)
            }
            this.onPlayerReady()
            this.onPlayerStateChange()
            this.resetProgress();
        },
        updateCurrentVideoInfo (index) {
            this.currentBackground = this.videos[index]?.thumbnail || ''
            this.currentTitle = this.videos[index]?.title || ''
        },
        playSongBtn() {
            if (this.videos.length > 0) {
                const firstVideo = this.videos[0]
                this.playVideo(firstVideo.id,0, firstVideo.thumbnail, firstVideo.title)
            } else {
                alert('Chưa có bài hát nào để phát!')
            }
        },
        togglePlayPause() {
            if (!this.player) return; // Thêm dòng này để đảm bảo player đã sẵn sàng

            if (this.isPlaying) {
                this.player.pauseVideo();
                this.pauseCdThumbAnimation();
                this.stopProgressTracking();
            } else {
                this.resetProgress();
                this.player.playVideo();
                this.startProgressTracking();
            }
            this.isPlaying = !this.isPlaying;
        },
        nextVideo() {
            if (this.videos.length === 0) return;
            let nextIndex;
            if (this.isRandom) {
                nextIndex = Math.floor(Math.random() * this.videos.length);
            } else {
                nextIndex = (this.currentVideoIndex + 1) % this.videos.length;
            }
            this.playVideoSafe(nextIndex);
        },
        prevVideo() {
            if (this.videos.length === 0) return;
            let prevIndex;
            if (this.isRandom) {
                prevIndex = Math.floor(Math.random() * this.videos.length);
            } else {
                prevIndex = (this.currentVideoIndex - 1 + this.videos.length) % this.videos.length;
            }
            this.playVideoSafe(prevIndex);
        },
        playVideoSafe(index) {
            const video = this.videos[index];
            if (video) {
                this.playVideo(video.id, index, video.thumbnail, video.title);
            }
        },
        toggleRandom() {
            this.isRandom = !this.isRandom
        },
        repeatVideo() {
            this.isRepeat = !this.isRepeat
        },
        initPlayer(videoId, index) {
            this.currentVideoIndex = index
            this.updateCurrentVideoInfo(index)
            if (window.YT && window.YT.Player) {
                this.player = new YT.Player('youtube-player', {
                videoId: videoId,
                playerVars: { autoplay: 0, controls: 0 },
                events: {
                    onReady: this.onPlayerReady,
                    onStateChange: this.onPlayerStateChange,
                }
                })
            } else {
                setTimeout(() => this.initPlayer(videoId, index), 500)
            }
        },
        onPlayerReady(event) {
            //event.target.playVideo()
            this.isPlaying = false;
        },
        onPlayerStateChange(event) {
            if (event.data === YT.PlayerState.ENDED) {
                if(this.isRepeat){
                    this.playVideo(this.currentVideoId,  this.currentVideoIndex, this.currentBackground, this.currentTitle);   
                    this.isPlaying = true
                    this.setBackgroundAnimation()
                    this.startProgressTracking()      
                }else {
                    this.nextVideo()
                    this.resetProgress();
                }
            } else if (event.data === YT.PlayerState.PLAYING) {
                this.isPlaying = true
                this.setBackgroundAnimation()
                this.startProgressTracking()
            } else if (event.data === YT.PlayerState.PAUSED) {
                this.isPlaying = false
                this.pauseCdThumbAnimation()
                this.stopProgressTracking()
            }
        },
        setBackgroundAnimation() {
            const cdThumb = this.$refs.cdThumb;
            if (cdThumb) {
                if (this.cdThumbAnimation) {
                    this.cdThumbAnimation.cancel(); 
                }
                this.cdThumbAnimation = cdThumb.animate([
                    { transform: 'rotate(0)' },
                    { transform: 'rotate(360deg)' }
                ], {
                    duration: 10000,
                    iterations: Infinity
                });
            }
        },
        pauseCdThumbAnimation() {
            if (this.cdThumbAnimation) {
                this.cdThumbAnimation.pause();
            }
        },
        resumeCdThumbAnimation() {
            if (this.cdThumbAnimation) {
                this.cdThumbAnimation.play();
            }
        },
        startProgressTracking() {
            if (this.progressInterval) clearInterval(this.progressInterval);
            this.progressInterval = setInterval(() => {
                if (this.player && this.isPlaying) {
                    const currentTime = this.player.getCurrentTime();
                    const duration = this.player.getDuration();
                    if (duration > 0) {
                        this.progressPercentage = (currentTime / duration) * 100;
                    }
                }
            }, 500);
        },
        stopProgressTracking() {
            if (this.progressInterval) {
            clearInterval(this.progressInterval);
            this.progressInterval = null;
            }
        },
        resetProgress() {
            this.progressPercentage = 0;
        },
    },
    mounted() {
        window.onYouTubeIframeAPIReady = this.initPlayerWrapper;
        this.fetchTrendingMusic();
    },
}
</script>
<style>

</style>