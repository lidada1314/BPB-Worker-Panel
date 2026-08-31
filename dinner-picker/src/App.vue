<script setup>
import { computed, ref } from 'vue';

const defaultFoods = [
  { name: '火锅', emoji: '🍲', reason: '热热闹闹，适合把一天的疲惫都煮掉。' },
  { name: '麻辣烫', emoji: '🥘', reason: '想吃什么夹什么，选择权和惊喜都在线。' },
  { name: '寿司', emoji: '🍣', reason: '清爽不油腻，今晚可以轻松一点。' },
  { name: '牛肉面', emoji: '🍜', reason: '一碗热汤面，简单、踏实、很满足。' },
  { name: '烤肉', emoji: '🥩', reason: '需要一点烟火气，也需要一点仪式感。' },
  { name: '披萨', emoji: '🍕', reason: '适合追剧、聊天，也适合奖励自己。' },
  { name: '米饭套餐', emoji: '🍱', reason: '有菜有肉有主食，稳定不容易踩雷。' },
  { name: '沙拉轻食', emoji: '🥗', reason: '清爽低负担，适合想早点休息的夜晚。' }
];

const foods = ref([...defaultFoods]);
const selectedFood = ref(defaultFoods[0]);
const customFood = ref('');
const isPicking = ref(false);

const foodCountText = computed(() => `${foods.value.length} 个候选`);

function pickDinner() {
  if (isPicking.value || foods.value.length === 0) return;

  isPicking.value = true;
  let count = 0;
  const timer = window.setInterval(() => {
    selectedFood.value = randomFood();
    count += 1;

    if (count >= 18) {
      window.clearInterval(timer);
      selectedFood.value = randomFood();
      isPicking.value = false;
    }
  }, 80);
}

function randomFood() {
  const index = Math.floor(Math.random() * foods.value.length);
  return foods.value[index];
}

function selectFood(food) {
  if (!isPicking.value) selectedFood.value = food;
}

function addFood() {
  const name = customFood.value.trim();
  if (!name || foods.value.some((food) => food.name === name)) return;

  const food = {
    name,
    emoji: '🍽️',
    reason: '这是你刚刚添加的心动选项。'
  };

  foods.value.push(food);
  selectedFood.value = food;
  customFood.value = '';
}

function resetFoods() {
  foods.value = [...defaultFoods];
  selectedFood.value = foods.value[0];
  customFood.value = '';
}
</script>

<template>
  <main class="page-shell">
    <section class="hero-card">
      <p class="eyebrow">Dinner Picker</p>
      <h1>晚上吃什么？</h1>
      <p class="subtitle">选择困难症救星：按一下，让今晚的菜单自己出现。</p>

      <div class="result-card" :class="{ 'is-picking': isPicking }">
        <span class="food-emoji" aria-hidden="true">{{ selectedFood.emoji }}</span>
        <div>
          <p class="result-label">今晚推荐</p>
          <h2>{{ selectedFood.name }}</h2>
          <p>{{ selectedFood.reason }}</p>
        </div>
      </div>

      <div class="actions">
        <button class="primary-button" type="button" :disabled="isPicking" @click="pickDinner">
          {{ isPicking ? '正在挑选...' : '帮我决定' }}
        </button>
        <button class="secondary-button" type="button" @click="resetFoods">重置菜单</button>
      </div>
    </section>

    <section class="menu-card" aria-labelledby="menu-title">
      <div class="menu-heading">
        <h2 id="menu-title">候选菜单</h2>
        <span>{{ foodCountText }}</span>
      </div>

      <form class="add-form" @submit.prevent="addFood">
        <label class="sr-only" for="custom-food">添加想吃的</label>
        <input id="custom-food" v-model="customFood" maxlength="20" placeholder="添加想吃的，比如烤鱼" />
        <button type="submit">添加</button>
      </form>

      <div class="food-list">
        <button
          v-for="food in foods"
          :key="food.name"
          class="food-chip"
          :class="{ active: food.name === selectedFood.name }"
          type="button"
          @click="selectFood(food)"
        >
          <span>{{ food.emoji }}</span>
          {{ food.name }}
        </button>
      </div>
    </section>
  </main>
</template>
