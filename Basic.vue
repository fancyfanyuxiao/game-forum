<script setup>
import { useUserStore } from '@/stores/user';
import { onMounted } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/swiper-bundle.css';

// 使用 Pinia 的 userStore
const userStore = useUserStore();

// 从 userStore 中获取数据
const { games, topGames, categories, selectedCategory, filteredGames } = userStore;

// Swiper 实例
let swiperInstance = null;

// 初始化 Swiper 实例
onMounted(() => {
    swiperInstance = document.querySelector('.custom-swiper').swiper;
});

// 切换到上一张
const slidePrev = () => {
    if (swiperInstance) {
        swiperInstance.slidePrev();
    }
};

// 切换到下一张
const slideNext = () => {
    if (swiperInstance) {
        swiperInstance.slideNext();
    }
};
</script>

<template>
    <div class="main-container">
        <!-- 轮播图 -->
        <div class="carousel">
            <Swiper :autoplay="{ delay: 3000 }" loop navigation class="custom-swiper">
                <SwiperSlide v-for="game in games" :key="game.id">
                    <div class="carousel-item">
                        <img :src="game.image" :alt="game.name" />
                        <p>{{ game.name }}</p>
                    </div>
                </SwiperSlide>
            </Swiper>
            <button class="swiper-button-prev" @click="slidePrev">&lt;</button>
            <button class="swiper-button-next" @click="slideNext">&gt;</button>
        </div>

        <!-- 排行榜 -->
        <div class="ranking">
            <h3>排行榜</h3>
            <ul>
                <li v-for="(game, index) in useUserStore().topGames" :key="game.id">
                    {{ index + 1 }}. {{ game.name }} ({{ game.likes }} 赞)
                </li>
            </ul>
        </div>

        <!-- 分类显示 -->
        <div class="categories">
            <h3>分类</h3>
            <select v-model="useUserStore().selectedCategory">
                <option v-for="category in useUserStore().categories" :key="category" :value="category">
                    {{ category }}
                </option>
            </select>
        </div>

        <div class="game-list">
            <div v-for="game in useUserStore().filteredGames" :key="game.id" class="game-item">
                <router-link :to="`/detail/${game.id}`">
                    <img :src="game.image" :alt="game.name" />
                </router-link>
                <p>{{ game.name }}</p>
            </div>
        </div>
        <router-view></router-view>
    </div>
</template>

<style scoped>
.main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background-color: #f9f9f9;
    font-family: Arial, sans-serif;
}

.carousel {
    width: 60%;
    margin-bottom: 30px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    position: relative;
}

.carousel-item {
    text-align: center;
    padding: 10px;
}

.carousel-item img {
    width: 100%;
    height: 250px;
    object-fit: cover;
    border-radius: 10px;
}

.carousel-item p {
    margin-top: 10px;
    font-size: 16px;
    color: #333;
}

.swiper-button-prev,
.swiper-button-next {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    cursor: pointer;
    z-index: 10;
}

.swiper-button-prev {
    left: 10px;
}

.swiper-button-next {
    right: 10px;
}

.ranking {
    position: absolute;
    top: 20px;
    right: 20px;
    background: #ffffff;
    padding: 15px;
    border: 1px solid #ddd;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    color: red;
    font-weight: bold;
}

.ranking ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.ranking li {
    margin: 5px 0;
    font-size: 14px;
}

.categories {
    margin-bottom: 20px;
    text-align: center;
}

.categories select {
    padding: 5px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #fff;
}

.game-list {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
}

.game-item {
    text-align: center;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 10px;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.game-item img {
    width: 150px;
    height: 100px;
    object-fit: cover;
    border-radius: 10px;
}

.game-item p {
    margin-top: 10px;
    font-size: 14px;
    color: #333;
}

.footer-links {
    margin-top: 30px;
    display: flex;
    gap: 20px;
    justify-content: center;
    font-size: 16px;
}

.footer-links a {
    text-decoration: none;
    color: #007bff;
    font-weight: bold;
}

.footer-links a:hover {
    text-decoration: underline;
}
</style>