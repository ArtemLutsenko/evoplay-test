<template>
  <div class="vendor-filter__list">
    <loader v-if="loading"></loader>
    <template v-else-if="activeFilter === 'alphabetically' && photoList.length ">
      <div class="vendor-filter__list-group" v-for="letter in listOfLetter"
           :key="letter"
      >
        <p class="vendor-filter__list-letter">{{letter}}</p>
        <vendor-filter-card  v-for="card in getFilteredList(letter)"
                          :key="card.id"
                          :id="card.id"
                          :title="card.title"
                          :image="card.thumbnailUrl"
                          :isFavorite="isFavorite(card.id)"
                          @toggleFavorite = toggleFavorite
        />
      </div>
    </template>
    <template v-else-if="activeFilter === 'favorite' && photoFavoriteList.length">
      <div class="vendor-filter__list-group">
        <vendor-filter-card v-for="card in photoFavoriteList"
                         :key="card.id"
                         :id="card.id"
                         :title="card.title"
                         :image="card.thumbnailUrl"
                         :isFavorite="isFavorite(card.id)"
                         @toggleFavorite = toggleFavorite
        />
      </div>
    </template>
    <template v-else>
      <div class="vendor-filter__empty">
        Данных нет
      </div>
    </template>
  </div>
</template>

<script>
import VendorFilterCard from '@/components/vendor-filter-card.vue';
import Loader from '@/components/loader.vue';

export default {
    name: 'vendor-filter-list',
    components: { Loader, VendorFilterCard },
    props: {
        activeFilter:{
            require: true,
            type: String,
        },
    },
    data () {
        return {
            loading: false,
            photoList: [],
            photoFavoriteList: [],
        };
    },

    computed: {
        listOfLetter () {
            const listOfAllLetter = this.photoList.map((item) => item.title[0]);

            return [...new Set(listOfAllLetter)];
        },
    },

    created () {
    },
    mounted () {
        this.fetchPhotoList();
    },

    methods: {
        async fetchPhotoList () {
            this.loading = true;
            try {
                const response = await fetch('http://jsonplaceholder.typicode.com/photos');
                const photoList = await response.json();
                this.photoList = photoList.slice(0, 100).sort((a, b) => {
                    if (a.title > b.title) return 1;
                    if (a.title < b.title) return -1;

                    return 0;
                });
            } catch (e) {
                console.log(e);
            } finally {
                this.loading = false;
            }
        },
        getFilteredList (letter) {
            return this.photoList.filter((item) => item.title.startsWith(letter));
        },
        toggleFavorite (id) {
            const indexOfElement = this.photoFavoriteList.map((el) => el.id).indexOf(id);
            if (indexOfElement < 0) {
                const element = this.photoList.find((el) => el.id === id);
                this.photoFavoriteList.push(element);
            } else {
                this.photoFavoriteList.splice(indexOfElement, 1);
            }
        },
        isFavorite (id) {
            return this.photoFavoriteList.some((el) => el.id === id);
        },
    },
};
</script>

