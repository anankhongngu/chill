<template>
    <div class="w3-row-padding d-flex">
        <div class="banner">
            <h1 class="banner-title">Manga</h1>
        </div>
        <div class="w3-col-25 w3-container  sticky">
            <div class="w3-margin-bottom">
                <div class="search-bar">
                    <button @click="searchManga" class="search-icon"><i class="fa fa-search"></i></button>
                    <input v-model="query" @keyup.enter="debouncedFetchManga" type="text" placeholder="Tìm kiếm truyện..." />
                </div> 
            </div>
        </div> 
        <div class="w3-col-75 w3-container">
            <div class="music-section w3-margin-bottom">       
                <div class="music-suggestions"  >
                    <h2>Kết quả tìm kiếm: {{  query }}</h2>
                    <div class="songs-list" v-if="mangaList.length">
                        <div class="song-item" v-for="(manga,index) in mangaList" :key="manga.id" 
                        :class="{ 'active': currentVideoId === manga.id }">
                            <router-link :to="`/manga/${manga.id}`">
                                <img :src="manga.cover" :alt="manga.title">
                                <div class="song-info">
                                    <p class="song-title">{{ manga.title }}</p>
                                    <p class="song-artist">{{ manga.description ? manga.description.slice(0, 100) + '...' : '...' }}</p>
                                </div>
                            </router-link>
                        </div>
                    </div>
                    <div class="songs-list empty-search" v-else>{{ textEmpty }}</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { debounce } from 'lodash';
import axios from 'axios';
export default {
    data() {
        return {
            query: '',
            mangaList: [],
            isSearch:  false,
            textEmpty: 'Nhập từ khóa để tìm truyện !!',
        };
    }, 
    watch: {
        query () {
            this.textEmpty = 'Đang tìm kiếm truyện.....';
            this.debouncedFetchManga();
        }
    },
    methods: {
        async searchManga() {
            if(!this.query) return; 

            this.textEmpty = 'Đang tìm kiếm truyện...';
            this.mangaList = [];

            const url = `https://api.mangadex.org/manga?title=${encodeURIComponent(this.query)}&limit=20&availableTranslatedLanguage[]=vi`;
            
            try {
                const response = await axios.get(url);
                if (response.data.total === 0) {
                    this.textEmpty = 'Không tìm thấy truyện theo yêu cầu!';
                    return;
                }
                const mangaDetailsPromises = response.data.data.map(manga =>
                    axios.get(`https://api.mangadex.org/manga/${manga.id}?includes[]=artist&includes[]=author&includes[]=cover_art&availableTranslatedLanguages[]=vi`)
                        .then(res => this.mangaData(manga.id, res.data.data))
                        .catch(() => null) 
                );
                const mangaDetails = (await Promise.all(mangaDetailsPromises)).filter(m => m);
                //console.log(mangaDetails)
                if (mangaDetails.length === 0) {
                    this.textEmpty = 'Không tìm thấy truyện phù hợp!';
                } else {
                    this.mangaList = mangaDetails;
                }
            } catch (error) {
                console.error('Lỗi khi gọi API:', error);
                this.isSearch = false;
            }
        },
        mangaData (mangaid,data) {
            const cover = data.relationships.find(item =>item.type === "cover_art")
            const coverUrl = cover ? `https://uploads.mangadex.org/covers/${mangaid}/${cover.attributes.fileName}` : '';

            return {
                id: mangaid,
                cover: coverUrl,
                title: this.getTitle(data.attributes),
                description: this.getDescription(data.attributes)
            };
        },
        // getCoverUrl (mangaId, relationships) {
        //     const cover = relationships.find(item => item.type === "cover_art");
        //     return cover ? `https://uploads.mangadex.org/covers/${mangaId}/${cover.attributes.fileName}` : '';
        // },
        getTitle (attributes) {
            return attributes.title.vi || attributes.title.en || attributes.title.ja || 'Chưa có tiêu đề';
        },
        getDescription (attributes) {
            return attributes.description.vi || attributes.description.en || attributes.description.ja || 'Đang cập nhật mô tả';
        },
        debouncedFetchManga: debounce(async function() {
            await this.searchManga();
        }, 1000)
    },
    mounted() {
        
    },
}
</script>
<style>

</style>