<template>
  <q-page padding class="menu-page">
    <section class="hero-grid animate__animated animate__fadeInUp">
      <div class="hero-panel">
        <q-chip color="primary" text-color="white" icon="auto_awesome" class="glass-chip">新紀元・指數學習港口</q-chip>
        <h1 class="page-title">沉浸式指數律體驗中心</h1>
        <p class="page-sub">
          全面清空過往版面，換上精品科技風的新介面。首頁即是你的導航儀表板，帶你進入互動式「指數律教學」舞台，
          感受動畫、粒子與光帶交織的學習氛圍。
        </p>
        <div class="hero-actions">
          <q-btn color="secondary" unelevated icon="electric_bolt" label="前往指數律教學" to="/exponent-laws" />
          <q-btn color="white" text-color="primary" flat icon="travel_explore" label="探索特色" @click="highlightFocus" />
        </div>
        <div class="status-strip" :class="{ active: focusActive }">
          <div class="status-label">動態互動模式</div>
          <div class="status-desc">動畫配色、互動卡片、數值控制與配對遊戲全面啟用</div>
        </div>
      </div>

      <div class="hero-visual">
        <div class="holo-card animate__animated animate__pulse animate__infinite">
          <div class="holo-header">
            <span class="tag">LUX</span>
            <span class="rarity">SSR</span>
          </div>
          <div class="holo-title">Exponent Royale Deck</div>
          <p class="holo-desc">高亮玻璃質感，導引每一步的指數冒險。</p>
          <div class="holo-links">
            <q-btn flat dense icon="apps" label="首頁選單" to="/" />
            <q-btn flat dense icon="auto_fix_high" label="互動教室" to="/exponent-laws" />
          </div>
        </div>
        <div class="floating-metrics">
          <div class="metric-card">
            <div class="metric-label">動畫佈景</div>
            <div class="metric-value">Animate.css + 玻璃霓虹</div>
          </div>
          <div class="metric-card">
            <div class="metric-label">體驗模式</div>
            <div class="metric-value">配對 / 數值重算 / 挑戰劇本</div>
          </div>
        </div>
      </div>
    </section>

    <section class="menu-grid animate__animated animate__fadeInUp animate__delay-1s">
      <q-card v-for="card in menuCards" :key="card.title" class="menu-card">
        <div class="card-header">
          <q-icon :name="card.icon" size="32px" color="secondary" />
          <div>
            <div class="card-title">{{ card.title }}</div>
            <div class="card-sub">{{ card.sub }}</div>
          </div>
        </div>
        <p class="card-desc">{{ card.desc }}</p>
        <div class="card-actions">
          <q-btn :to="card.to" color="secondary" rounded unelevated :label="card.action" />
        </div>
      </q-card>
    </section>

    <section class="callout animate__animated animate__fadeInUp animate__delay-2s">
      <div class="callout-text">
        <div class="callout-title">豪華精品科技風</div>
        <p>
          主色採用夜空藍與霓虹青，搭配玻璃質感邊框與微動態光暈。所有入口均導向全新的指數律互動教學，
          無需翻找舊頁面，即刻啟航。
        </p>
      </div>
      <div class="callout-actions">
        <q-btn color="white" text-color="primary" flat icon="rocket_launch" label="立即啟動教學" to="/exponent-laws" />
      </div>
    </section>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface MenuCard {
  title: string;
  sub: string;
  desc: string;
  icon: string;
  to: string;
  action: string;
}

const focusActive = ref(false);

const menuCards: MenuCard[] = [
  {
    title: '指數律教學主舞台',
    sub: '動態卡片・數值操控・公式配對',
    desc: '進入動畫與互動並重的專屬舞台，逐條揭開指數律的奧義，並搭配數值重算、挑戰劇本等遊戲化體驗。',
    icon: 'electric_bolt',
    to: '/exponent-laws',
    action: '前往教學',
  },
  {
    title: '豪華首頁選單',
    sub: '清空重製・精品科技風',
    desc: '所有入口集中於此，使用霓虹質感與滑入動畫營造高階科技展館，隨時回到起點。',
    icon: 'dashboard_customize',
    to: '/',
    action: '留在首頁',
  },
];

function highlightFocus() {
  focusActive.value = true;
  setTimeout(() => {
    focusActive.value = false;
  }, 1200);
}
</script>
