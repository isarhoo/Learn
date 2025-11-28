<template>
  <q-page padding class="flex flex-center">
    <div class="column q-gutter-md items-center">
      <q-card class="q-pa-lg hello-card">
        <q-card-section class="text-center">
          <div class="text-h4 text-primary">哈囉，世界！</div>
          <div class="text-subtitle1 text-grey-8 q-mt-sm">
            歡迎來到新的 Quasar + Vue 3 專案，並已準備好部署到 GitHub Pages。
          </div>
        </q-card-section>
        <q-separator inset />
        <q-card-section>
          <div class="text-body1 q-mb-sm">
            Quasar 元件已可直接使用，按下方的按鈕會觸發波紋效果，確認環境設定正確。
          </div>
          <q-btn color="primary" label="啟動" icon="rocket_launch" unelevated />
        </q-card-section>
      </q-card>

      <q-banner rounded class="bg-secondary text-white shadow-2" inline-actions>
        <div class="text-subtitle1">Quasar 介面已啟用。</div>
        <template #action>
          <q-btn flat color="white" label="OK" icon="check_circle" />
        </template>
      </q-banner>
    </div>
  </q-page>
</template>

<style scoped lang="scss">
.hello-card {
  max-width: 640px;
}
</style>
