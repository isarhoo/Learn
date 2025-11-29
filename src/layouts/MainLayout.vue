<template>
  <q-layout view="lHh Lpr lFf" class="lux-layout">
    <div class="orb orb-1" />
    <div class="orb orb-2" />
    <div class="orb orb-3" />

    <q-header elevated class="glass-header animate__animated animate__fadeInDown">
      <q-toolbar class="toolbar">
        <q-btn flat dense round icon="auto_awesome" color="white" aria-label="logo" />
        <div class="brand">
          <div class="brand-title">Exponent Royale</div>
          <div class="brand-sub">豪華精品科技風互動學院</div>
        </div>
        <q-space />
        <q-btn flat rounded icon="apps" label="首頁選單" to="/" class="nav-btn" />
        <q-btn flat rounded icon="electric_bolt" label="指數律教學" to="/exponent-laws" class="nav-btn" />
      </q-toolbar>
    </q-header>

    <q-page-container class="page-shell">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts"></script>
