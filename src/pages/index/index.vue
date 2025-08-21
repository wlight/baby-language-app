<template>
  <view class="container">
    <view class="title">{{ title }}</view>
    <view class="category-grid">
      <view
        v-for="category in categories"
        :key="category.id"
        class="category-card"
        @click="goToCategory(category)"
      >
        <image :src="category.icon_path" class="category-icon" />
        <text class="category-name">{{ category.name }}</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';
import contentData from '../../data/content.json';

const title = ref('宝宝学语言');
const categories = ref(contentData.map(item => item.category));

const goToCategory = (category) => {
  uni.navigateTo({
    url: `/pages/category/index?id=${category.id}&title=${category.name}`
  });
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 20px;
  background-color: #f0f8ff;
  min-height: 100vh;
  box-sizing: border-box;
  overflow: hidden;
}

.title {
  font-size: 32px;
  font-weight: bold;
  color: #333;
  margin-bottom: 40px;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 30px;
  width: 100%;
  max-width: 800px;
  padding: 0 20px;
}

.category-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  aspect-ratio: 1/1;
  background-color: #ffffff;
  border-radius: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.category-card:active {
  transform: scale(0.95);
}

.category-icon {
  width: 80px;
  height: 80px;
  margin-bottom: 10px;
}

.category-name {
  font-size: 18px;
  color: #555;
}

/* iPad specific styles */
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  .category-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 40px;
  }
  
  .title {
    font-size: 40px;
    margin-bottom: 60px;
  }
  
  .category-icon {
    width: 100px;
    height: 100px;
  }
  
  .category-name {
    font-size: 22px;
  }
}
</style>
