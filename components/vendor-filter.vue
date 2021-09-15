<template>
    <div class="vendor-filter">
        <div class="vendor-filter__header">
            <div class="vendor-filter__title">
                Фильтр:
                <span class="vendor-filter__name"
                      :class="{'active': activeFilter === 'alphabetically'}"
                      @click="changeSortType('alphabetically')">По альбомам</span>
                <span class="vendor-filter__name"
                      :class="{'active': activeFilter === 'favorite'}"
                      @click="changeSortType('favorite')">Избранное</span>
            </div>
        </div>
        <div class="vendor-filter__scroll-wrap">
            <div class="vendor-filter__scroll">
              <vendor-filter-list :activeFilter="activeFilter"/>
            </div>
        </div>
    </div>
</template>

<script>

import VendorFilterList from '@/components/vendor-filter-list.vue';

export default {
    components: { VendorFilterList },
    data () {
        return {
            activeFilter: localStorage.modalPhotoFilter || 'alphabetically',
        };
    },
    methods: {
        changeSortType (sortType) {
            if (this.activeFilter !== sortType) {
                this.activeFilter = sortType;
                localStorage.modalPhotoFilter = sortType;
            }
        },
    },
};
</script>

<style lang="scss">
.vendor-filter {
    font-weight: 700;
    font-family: Open Sans, Roboto, sans-serif;
    width: 1300px;
    height: 600px;
    margin: auto;
    padding: 3.6rem 4.8rem 6rem;
    font-size: 1.2rem;
    background: #ffffff;
    border-radius: 1.2rem;
    box-sizing: border-box;

    &__header {
        margin: 0 0 1.5rem;
    }

    &__title {
        font-weight: bold;
        font-size: 1.4rem;
        line-height: 1.9rem;
    }

    &__name {
        margin: 0 0.5rem;
        text-decoration: underline;
        cursor: pointer;
    }

    &__name.active {
        color: #3e5ef2;
    }

    &__scroll-wrap {
        overflow: hidden;
    }

    &__scroll {
        width: 100%;
        max-height: 52rem;
        overflow-y: auto;
    }

    &__list {
        position: relative;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        max-height: 1670px;

        &-letter {
            font-weight: bold;
            font-size: 12px;
            line-height: 16px;
            padding-left: 20px;
            text-transform: uppercase;
            margin-bottom: 8px;
        }
    }
}
</style>
