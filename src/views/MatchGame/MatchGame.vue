<script setup>
import { ref } from 'vue';
import Card from './components/Card.vue';

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const cards = ref([]);
const openedCard = ref([]);

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
}

const clickHandler = (idx) => {
  openedCard.value.push(idx);

  if (openedCard.value.length === 2) {
    setTimeout(() => {
      if (cards.value[openedCard.value[0]] === cards.value[openedCard.value[1]]) {
        // 兩張牌面相同，牌面消失
        cards.value[openedCard.value[0]] = 0;
        cards.value[openedCard.value[1]] = 0;
      }
      openedCard.value = [];

      // Move the check here, inside the setTimeout
      checkGameEnd();
    }, 1000);
  }
}

// Separate function to check game end
const checkGameEnd = () => {
  if (cards.value.every(card => card === 0)) {
    alert('恭喜破關，再來一局？');
    gameInit();
  }
}
</script>

<template>
  <div class="bg-emerald-900 min-h-screen w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
      <Card
        v-for="(n, idx) in cards"
        :cardNumber="n"
        :isOpen="openedCard.includes(idx)"
        :isVisible="n > 0"
        :onClick="() => clickHandler(idx)"
      />
    </div>
  </div>
</template>

<style src="./MatchGame.css"></style>