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
  justify-content: center;
  padding: 40px 20px;
  background-color: #f0f8ff;
  height: 100vh;
}

.title {
  font-size: 32px;
  font-weight: bold;
  color: #333;
  margin-bottom: 40px;
}

.category-grid {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 25px;
}

.category-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 150px;
  height: 150px;
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
</style>
