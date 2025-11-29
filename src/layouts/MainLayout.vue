<template>
  <q-layout view="lHh Lpr lFf" class="card-ui">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>指數決鬥場</q-toolbar-title>

        <q-chip dense color="secondary" text-color="white" icon="auto_awesome">
          Quasar v{{ $q.version }}
        </q-chip>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered class="card-drawer">
      <div class="drawer-banner">
        <div class="banner-title">公式冒險選單</div>
        <div class="banner-sub">挑選你的下一場戰鬥</div>
      </div>
      <q-list padding separator>
        <q-item clickable v-ripple to="/">
          <q-item-section avatar>
            <q-icon name="home" />
          </q-item-section>
          <q-item-section>
            <q-item-label>主選單</q-item-label>
            <q-item-label caption>返回對戰大廳</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable v-ripple to="/exponent-laws">
          <q-item-section avatar>
            <q-icon name="auto_fix_high" />
          </q-item-section>
          <q-item-section>
            <q-item-label>指數律卡牌圖鑑</q-item-label>
            <q-item-label caption>依序開啟每張效果卡</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>
