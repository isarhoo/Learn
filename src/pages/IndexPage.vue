<template>
  <q-page padding class="menu-page">
    <div class="hero">
      <div class="hero-text">
        <div class="label">指數訓練家大廳</div>
        <div class="title">組合你的效果卡，開啟指數冒險！</div>
        <div class="subtitle">
          這裡是所有挑戰的入口，先檢查套牌，再進入「指數律卡牌圖鑑」面對每條公式的特性。
        </div>
        <div class="hero-actions">
          <q-btn color="secondary" unelevated icon="auto_fix_high" label="進入指數律卡牌" to="/exponent-laws" />
          <q-btn color="white" text-color="primary" flat icon="menu_book" label="快速總覽" @click="showTips = !showTips" />
        </div>
        <q-slide-transition>
          <div v-show="showTips" class="tip-box">
            <div class="tip-title">教練提醒</div>
            <ul>
              <li>每張卡片都代表一條指數律，點擊後可查看詳細效果與範例。</li>
              <li>大廳維持遊戲風格，隨時可從左側選單切換戰場。</li>
            </ul>
          </div>
        </q-slide-transition>
      </div>
      <div class="hero-card poke-card">
        <div class="card-glow" />
        <div class="card-content">
          <div class="card-header">
            <span class="energy">EXP</span>
            <span class="rarity">SR</span>
          </div>
          <div class="card-title">指數律召喚</div>
          <div class="card-desc">點擊下方按鈕即可啟動對戰模式，完整收錄所有常見的指數運算公式。</div>
          <q-btn color="primary" icon="flash_on" label="開始對戰" to="/exponent-laws" class="full-btn" />
        </div>
      </div>
    </div>

    <div class="deck-grid">
      <div class="deck-card" v-for="item in deckOptions" :key="item.title">
        <div class="deck-header">
          <q-icon :name="item.icon" />
          <span>{{ item.title }}</span>
        </div>
        <div class="deck-body">{{ item.desc }}</div>
        <div class="deck-footer">
          <q-btn :color="item.actionColor" :to="item.to" :label="item.action" dense flat icon="chevron_right" />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface DeckCard {
  title: string;
  desc: string;
  icon: string;
  to: string;
  action: string;
  actionColor: string;
}

const showTips = ref(false);

const deckOptions: DeckCard[] = [
  {
    title: '指數律卡牌圖鑑',
    desc: '查看所有指數公式效果、戰術提示與計算範例，熟悉每一張卡。',
    icon: 'collections_bookmark',
    to: '/exponent-laws',
    action: '立即查看',
    actionColor: 'secondary',
  },
  {
    title: '主選單',
    desc: '保留在大廳等待下一場對戰，從左側選單切換其他章節。',
    icon: 'home',
    to: '/',
    action: '留在大廳',
    actionColor: 'primary',
  },
];
</script>
