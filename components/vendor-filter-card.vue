<template>
  <div class="card">
    <img :src="image" alt="" class="card-image">
    <p class="card-title">{{ title }}</p>
    <div
        class="card-star"
        :class="{'favorite': isFavorite}"
        @click="toggleFavorite"></div>
  </div>
</template>

<script>
export default {
    name: 'photo-list-card',
    props: {
        title: {
            require: true,
            type: String,
        },
        image: {
            require: true,
            type: String,
        },
        id: {
            require: true,
            type: Number,
        },
        isFavorite: {
            require: false,
            default: false,
            type: Boolean,
        }
    },
    methods: {
        toggleFavorite () {
            this.$emit('toggleFavorite', this.id);
        },
    },
};
</script>

<style scoped lang="scss">
.card {
    display: flex;
    align-items: center;
    width: 260px;
    height: 40px;
    background: #F7F8F9;
    border-radius: 8px;
    padding: 4px 40px 4px 4px;
    margin-bottom: 8px;
    position: relative;
    box-sizing: border-box;

    &:hover {
        cursor: default;
    }

    &-image {
        width: 32px;
        height: 32px;
        border-radius: 8px;
        margin-right: 11px;
    }

    &-title {
        font-weight: bold;
        font-size: 12px;
        line-height: 16px;
        color: #1C214C;
        overflow: hidden;
        white-space:nowrap;
        text-overflow: ellipsis;
        width: 173px;
    }

    &-star {
        background: url('../static/img/star.svg');
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        right: 15px;
        width: 14px;
        height: 14px;

        &:hover {
            cursor: pointer;
        }
    }

    &-star.favorite {
        background: url('../static/img/star-active.svg');
    }
}
</style>
