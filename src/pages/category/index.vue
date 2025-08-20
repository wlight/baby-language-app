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

const playSounds = (card) => {
  if (innerAudioContext) {
    innerAudioContext.destroy();
  }
  
  innerAudioContext = uni.createInnerAudioContext();
  
  // 播放真实音效 (如果存在)
  if (card.effect_sound_path) {
    innerAudioContext.src = card.effect_sound_path;
    innerAudioContext.play();
    innerAudioContext.onEnded(() => {
      // 音效播放结束后播放名称
      innerAudioContext.src = card.name_sound_path;
      innerAudioContext.play();
    });
  } else {
    // 如果没有音效，直接播放名称
    innerAudioContext.src = card.name_sound_path;
    innerAudioContext.play();
  }
};
</script>

<style>
.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 20px;
  gap: 20px;
}

.card-item {
  width: 45%;
  aspect-ratio: 1 / 1; /* 保持宽高比为1:1 */
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
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
  margin-top: 10px;
  font-size: 16px;
  color: #333;
}
</style>