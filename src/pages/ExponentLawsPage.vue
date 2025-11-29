<template>
  <q-page class="exponent-page" padding>
    <div class="intro-card">
      <div class="intro-text">
        <div class="label">指數律卡牌圖鑑</div>
        <div class="title">每張效果卡都藏著指數運算的秘密</div>
        <div class="subtitle">
          點擊卡片即可翻開，查看公式、意義與例題。把它們組合起來，就能在任何計算戰場快速出招！
        </div>
        <div class="badges">
          <q-chip color="secondary" text-color="white" icon="bolt">核心公式</q-chip>
          <q-chip color="primary" text-color="white" icon="emoji_objects">戰術提示</q-chip>
          <q-chip color="amber" text-color="black" icon="functions">計算實例</q-chip>
        </div>
      </div>
      <div class="intro-illustration poke-card">
        <div class="card-glow" />
        <div class="card-content">
          <div class="card-header">
            <span class="energy">EXP</span>
            <span class="rarity">UR</span>
          </div>
          <div class="card-title">展開卡包</div>
          <div class="card-desc">合體、拆解、轉換底數，每一個動作都是一次策略選擇。</div>
          <q-btn color="secondary" icon="unfold_more" label="全選展開" class="full-btn" @click="expandAll" />
        </div>
      </div>
    </div>

    <div class="card-grid">
      <div
        v-for="rule in rules"
        :key="rule.id"
        class="poke-card rule-card"
        :class="{ active: openedCards.has(rule.id) }"
        @click="toggle(rule.id)"
      >
        <div class="card-glow" />
        <div class="card-content">
          <div class="card-header">
            <span class="energy">{{ rule.tag }}</span>
            <span class="rarity">{{ rule.rarity }}</span>
          </div>
          <div class="card-title">{{ rule.title }}</div>
          <div class="formula">{{ rule.formula }}</div>
          <q-slide-transition>
            <div v-show="openedCards.has(rule.id)" class="details">
              <div class="detail-row">
                <q-icon name="tips_and_updates" color="amber" size="24px" />
                <div>{{ rule.description }}</div>
              </div>
              <div class="detail-row example">
                <q-icon name="calculate" color="light-blue" size="24px" />
                <div>
                  <div class="example-title">範例</div>
                  <div>{{ rule.example }}</div>
                </div>
              </div>
              <div class="detail-row hint">
                <q-icon name="sports_esports" color="secondary" size="24px" />
                <div>{{ rule.tip }}</div>
              </div>
            </div>
          </q-slide-transition>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface RuleCard {
  id: string;
  title: string;
  formula: string;
  description: string;
  example: string;
  tip: string;
  rarity: string;
  tag: string;
}

const rules: RuleCard[] = [
  {
    id: 'multiply',
    title: '同底數相乘',
    formula: 'a^m · a^n = a^(m + n)',
    description: '同樣底數的次方相乘時，指數可以直接相加，就像把能量合併成更強的一擊。',
    example: '2^3 · 2^4 = 2^(3+4) = 2^7 = 128',
    tip: '當係數也需要相乘時，先處理數字再合併指數，能讓計算更乾脆。',
    rarity: 'SR',
    tag: 'Combine',
  },
  {
    id: 'divide',
    title: '同底數相除',
    formula: 'a^m ÷ a^n = a^(m - n)',
    description: '相同底數的商會以指數相減留下的力量表現，如果結果是負數代表力量被轉到分母。',
    example: '5^6 ÷ 5^2 = 5^(6−2) = 5^4 = 625',
    tip: '習慣在紙上標註「剩餘指數」能避免錯把指數當係數去相減。',
    rarity: 'HR',
    tag: 'Counter',
  },
  {
    id: 'power-of-power',
    title: '冪的冪',
    formula: '(a^m)^n = a^(m · n)',
    description: '重複乘上同一個冪次，就等於把指數相乘，像是連續疊加的增幅。',
    example: '(3^2)^3 = 3^(2·3) = 3^6 = 729',
    tip: '如果還有係數，記得也要套用括號內的次方，避免漏掉倍數。',
    rarity: 'UR',
    tag: 'Boost',
  },
  {
    id: 'power-of-product',
    title: '積的冪',
    formula: '(ab)^n = a^n · b^n',
    description: '括號內的每個因數都會被同一個指數強化，拆開後再相乘會更清晰。',
    example: '(2·3)^4 = 2^4 · 3^4 = 16 · 81 = 1296',
    tip: '用來簡化大括號時很好用，先拆開再把結果合併能避免乘法過程出錯。',
    rarity: 'SR',
    tag: 'Split',
  },
  {
    id: 'power-of-quotient',
    title: '商的冪',
    formula: '(a/b)^n = a^n ÷ b^n',
    description: '分子與分母都會同時受到指數影響，拆開後檢查是否能約分或化簡。',
    example: '(4/5)^3 = 4^3 ÷ 5^3 = 64/125',
    tip: '記得完整套用到分子與分母，並保留分母避免不必要的小數。',
    rarity: 'SR',
    tag: 'Balance',
  },
  {
    id: 'zero-exponent',
    title: '零次方',
    formula: 'a^0 = 1 (a ≠ 0)',
    description: '任何非零數字的零次方都會收斂成 1，像是回到原點的重置按鈕。',
    example: '7^0 = 1，(-3)^0 = 1',
    tip: '常見在化簡後剩下的項目，不要忘記它仍然代表 1 而非 0。',
    rarity: 'R',
    tag: 'Reset',
  },
  {
    id: 'negative-exponent',
    title: '負指數',
    formula: 'a^(-n) = 1 / a^n',
    description: '負指數代表把力量翻到分母，先轉換成分數再運算會更好掌握。',
    example: '2^-3 = 1 / 2^3 = 1/8',
    tip: '遇到連乘或分式時，先把所有負指數移到相反的位置再合併。',
    rarity: 'HR',
    tag: 'Invert',
  },
  {
    id: 'fractional-exponent',
    title: '分數指數',
    formula: 'a^(m/n) = n√(a^m)',
    description: '分數指數同時代表根號與次方，分母是開根次數，分子是對結果再進行次方。',
    example: '8^(2/3) = ³√(8^2) = ³√64 = 4',
    tip: '先處理根號或先處理次方都可以，但務必保持根號次數與指數一致。',
    rarity: 'UR',
    tag: 'Dual',
  },
];

const openedCards = ref<Set<string>>(new Set());

function toggle(id: string) {
  if (openedCards.value.has(id)) {
    openedCards.value.delete(id);
  } else {
    openedCards.value.add(id);
  }
  // 觸發響應
  openedCards.value = new Set(openedCards.value);
}

function expandAll() {
  openedCards.value = new Set(rules.map((rule) => rule.id));
}
</script>
