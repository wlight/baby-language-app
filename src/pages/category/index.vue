<template>
  <view class="card-container">
    <view
      v-for="card in cards"
      :key="card.id"
      class="card-item"
      @click="playSounds(card)"
    >
      <image :src="card.image_path" class="card-image" />
      <text class="card-name">{{ card.name }}</text>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';
import { onLoad } from '@dcloudio/uni-app';
import contentData from '../../data/content.json';

const cards = ref([]);
const categoryTitle = ref('');

let innerAudioContext = null;
let playCount = 0;
let playCompleted = false;

onLoad((options) => {
  if (options.id) {
    uni.setNavigationBarTitle({
      title: options.title
    });

    const categoryData = contentData.find(item => item.category.id == options.id);
    if (categoryData) {
      cards.value = categoryData.cards;
    }
  }
});

const showFullScreenImage = (card) => {
  // 创建全屏遮罩
  const mask = document.createElement('div');
  mask.style.position = 'fixed';
  mask.style.top = '0';
  mask.style.left = '0';
  mask.style.width = '100%';
  mask.style.height = '100%';
  mask.style.backgroundColor = 'rgba(0,0,0,0.8)';
  mask.style.zIndex = '9999';
  mask.style.display = 'flex';
  mask.style.justifyContent = 'center';
  mask.style.alignItems = 'center';
  
  // 创建图片容器
  const img = document.createElement('img');
  img.src = card.image_path;
  img.style.maxWidth = '100%';
  img.style.maxHeight = '100%';
  img.style.objectFit = 'contain';
  
  mask.appendChild(img);
  document.body.appendChild(mask);
  
  // 3秒后自动关闭
  setTimeout(() => {
    document.body.removeChild(mask);
  }, 3000);
  
  // 点击关闭
  mask.onclick = () => {
    document.body.removeChild(mask);
  };
};

const playSounds = (card) => {
  showFullScreenImage(card);

  // 如果没有名称音频则直接返回
  if (!card.name_sound_path) {
    return;
  }

  if (innerAudioContext) {
    innerAudioContext.destroy();
  }
  
  innerAudioContext = uni.createInnerAudioContext();
  innerAudioContext.src = card.name_sound_path;
  innerAudioContext.play();
};
</script>

<style>
.card-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 20px;
  padding: 15px;
  max-width: 100%;
  margin: 0 auto;
  box-sizing: border-box;
  overflow-x: hidden;
}

.card-item {
  width: 100%;
  aspect-ratio: 1 / 1;
  background-color: #fff;
  border-radius: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s;
}

.card-item:active {
  transform: scale(0.95);
}

.card-image {
  width: 70%;
  height: 70%;
  object-fit: contain;
}

.card-name {
  margin-top: 15px;
  font-size: 18px;
  color: #333;
  font-weight: 500;
}

/* iPad specific styles */
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  .card-container {
    grid-template-columns: repeat(3, 1fr);
    gap: 40px;
    padding: 40px;
  }
  
  .card-item {
    border-radius: 25px;
  }
  
  .card-image {
    width: 75%;
    height: 75%;
  }
  
  .card-name {
    font-size: 22px;
    margin-top: 20px;
  }
}
</style>