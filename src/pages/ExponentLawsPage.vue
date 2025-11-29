<template>
  <q-page padding class="exponent-lab">
    <section class="lab-hero animate__animated animate__fadeIn" :class="{ animate__flash: heroAnimated }">
      <div>
        <q-chip color="secondary" text-color="white" icon="stadia_controller" class="glass-chip"
          >指數律互動教室</q-chip
        >
        <h1 class="section-title">遊戲化指數律攻略</h1>
        <p class="section-sub">
          每條公式都化身高稀有度卡片，透過配對、動態數值與挑戰劇本，讓公式不再只是記憶，而是可操作、可感受的戰術。
          使用 Animate.css 製作的浮動效果與 Quasar 元件，營造豪華科技感。
        </p>
        <div class="hero-actions">
          <q-btn color="secondary" icon="flash_on" unelevated label="同步動畫" @click="flashHero" />
          <q-btn color="white" text-color="primary" flat icon="replay" label="重置配對" @click="resetPairing" />
        </div>
      </div>
      <div class="lab-stats">
        <div class="stat-card">
          <div class="stat-title">完成配對</div>
          <div class="stat-value">{{ matchedIds.size }} / {{ pairingCards.length }}</div>
          <q-linear-progress
            size="12px"
            rounded
            color="secondary"
            :value="matchedIds.size / pairingCards.length"
            class="q-mt-sm"
          />
        </div>
        <div class="stat-card">
          <div class="stat-title">動畫狀態</div>
          <div class="stat-value">{{ heroAnimated ? '閃動中' : '待命' }}</div>
          <div class="stat-desc">Animate.css 驅動的霓虹呼吸燈效</div>
        </div>
      </div>
    </section>

    <section class="interaction-lab animate__animated animate__fadeInUp">
      <div class="panel-header">
        <div>
          <div class="panel-title">數值控制台</div>
          <div class="panel-sub">調整底數與指數，立即重算各條指數律的結果</div>
        </div>
        <q-btn flat dense icon="casino" label="亂數重點" @click="rollDemoNumbers" />
      </div>
      <div class="control-grid">
        <div class="control-card">
          <div class="control-row">
            <span>底數</span>
            <q-slider v-model="base" :min="2" :max="9" color="secondary" label-always />
          </div>
          <div class="control-row">
            <span>指數 A</span>
            <q-slider v-model="expOne" :min="1" :max="8" color="primary" label-always />
          </div>
          <div class="control-row">
            <span>指數 B</span>
            <q-slider v-model="expTwo" :min="1" :max="8" color="accent" label-always />
          </div>
          <div class="control-row">
            <span>根號分母 (分數指數)</span>
            <q-slider v-model="root" :min="2" :max="6" color="amber" label-always />
          </div>
        </div>
        <div class="result-card">
          <div class="result-line" v-for="item in computedList" :key="item.title">
            <div>
              <div class="result-title">{{ item.title }}</div>
              <div class="result-formula">{{ item.formula }}</div>
            </div>
            <q-chip color="secondary" text-color="white" class="result-chip">{{ item.value }}</q-chip>
          </div>
        </div>
      </div>
    </section>

    <section class="pairing-zone animate__animated animate__fadeInUp animate__delay-1s">
      <div class="panel-header">
        <div>
          <div class="panel-title">公式配對試煉</div>
          <div class="panel-sub">選一張公式卡，再點擊對應的敘述卡，完成配對累積進度。</div>
        </div>
        <q-btn flat dense icon="shuffle" label="重洗敘述卡" @click="shuffleMeanings" />
      </div>
      <div class="pair-grid">
        <div
          v-for="card in pairingCards"
          :key="card.id"
          class="pair-card"
          :class="{
            active: activeFormula === card.id,
            matched: matchedIds.has(card.id),
            'animate__animated animate__pulse': matchedIds.has(card.id),
          }"
          @click="selectFormula(card.id)"
        >
          <div class="pair-title">{{ card.name }}</div>
          <div class="pair-formula">{{ card.formula }}</div>
          <div class="pair-rarity">{{ card.tag }}</div>
        </div>
      </div>
      <div class="meaning-grid">
        <q-chip
          v-for="meaning in meaningPool"
          :key="meaning.id + meaning.text"
          clickable
          class="meaning-chip"
          :color="matchedIds.has(meaning.id) ? 'green' : 'white'"
          :text-color="matchedIds.has(meaning.id) ? 'white' : 'primary'"
          @click="chooseMeaning(meaning.id)"
        >
          {{ meaning.text }}
        </q-chip>
      </div>
      <div class="feedback" :class="{ success: feedbackType === 'success', error: feedbackType === 'error' }">
        {{ feedbackMessage }}
      </div>
    </section>

    <section class="scenario-zone animate__animated animate__fadeInUp animate__delay-2s">
      <div class="panel-header">
        <div>
          <div class="panel-title">挑戰劇本</div>
          <div class="panel-sub">隨機生成題型，先依提示判斷公式，再檢查答案與策略。</div>
        </div>
        <div class="scenario-actions">
          <q-btn flat dense icon="restart_alt" label="換一題" @click="generateScenario" />
          <q-btn flat dense icon="visibility" label="揭露答案" @click="showAnswer = !showAnswer" />
        </div>
      </div>
      <div class="scenario-card">
        <div class="scenario-title">{{ currentScenario.title }}</div>
        <div class="scenario-desc">{{ currentScenario.prompt }}</div>
        <div class="scenario-meta">
          <q-chip color="secondary" text-color="white" icon="category">{{ currentScenario.rule }}</q-chip>
          <q-chip color="amber" text-color="black" icon="calculate">底數 {{ currentScenario.base }}，指數 {{ currentScenario.expA }} / {{ currentScenario.expB }}</q-chip>
        </div>
        <q-slide-transition>
          <div v-show="showAnswer" class="scenario-answer">
            <div class="answer-label">答案</div>
            <div class="answer-value">{{ currentScenario.answer }}</div>
            <div class="answer-extra">策略：{{ currentScenario.strategy }}</div>
          </div>
        </q-slide-transition>
      </div>
    </section>
  </q-page>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';

interface PairCard {
  id: string;
  name: string;
  formula: string;
  tag: string;
}

interface MeaningItem {
  id: string;
  text: string;
}

interface Scenario {
  title: string;
  prompt: string;
  base: number;
  expA: number;
  expB: number;
  rule: string;
  answer: string;
  strategy: string;
}

const base = ref(3);
const expOne = ref(2);
const expTwo = ref(3);
const root = ref(2);

const heroAnimated = ref(false);
const activeFormula = ref<string | null>(null);
const matchedIds = ref<Set<string>>(new Set());
const feedbackMessage = ref('請選擇一張公式卡，再配對對應的敘述卡。');
const feedbackType = ref<'neutral' | 'success' | 'error'>('neutral');
const showAnswer = ref(false);

const pairingCards: PairCard[] = [
  { id: 'multiply', name: '同底數相乘', formula: 'a^m · a^n = a^(m+n)', tag: 'Combine' },
  { id: 'divide', name: '同底數相除', formula: 'a^m ÷ a^n = a^(m-n)', tag: 'Counter' },
  { id: 'power', name: '冪的冪', formula: '(a^m)^n = a^(m·n)', tag: 'Boost' },
  { id: 'product', name: '積的冪', formula: '(ab)^n = a^n · b^n', tag: 'Split' },
  { id: 'quotient', name: '商的冪', formula: '(a/b)^n = a^n ÷ b^n', tag: 'Balance' },
  { id: 'zero', name: '零次方', formula: 'a^0 = 1 (a ≠ 0)', tag: 'Reset' },
  { id: 'negative', name: '負指數', formula: 'a^(-n) = 1/a^n', tag: 'Invert' },
  { id: 'fraction', name: '分數指數', formula: 'a^(m/n) = n√(a^m)', tag: 'Dual' },
];

const meaningPool = ref<MeaningItem[]>([]);
const scenarioTemplates = [
  '將同底數的兩個能量疊加，輸出更強一擊。',
  '利用除法留下剩餘指數，守住能量核心。',
  '連續疊加強化，每一次冪次都是倍增。',
  '拆解乘法成分，逐一強化再合體。',
  '先拆解分子分母的力量，再判斷是否能約分。',
  '按下重置鍵，讓力量歸於 1。',
  '把能量翻到對面，形成倒數的防禦姿態。',
  '根號與次方雙重任務，同步完成。',
];

const computedList = computed(() => [
  {
    title: '同底數相乘',
    formula: `${base.value}^${expOne.value} · ${base.value}^${expTwo.value}`,
    value: formatNumber(Math.pow(base.value, expOne.value + expTwo.value)),
  },
  {
    title: '同底數相除',
    formula: `${base.value}^${expOne.value} ÷ ${base.value}^${expTwo.value}`,
    value: formatNumber(Math.pow(base.value, expOne.value - expTwo.value)),
  },
  {
    title: '冪的冪',
    formula: `(${base.value}^${expOne.value})^${expTwo.value}`,
    value: formatNumber(Math.pow(base.value, expOne.value * expTwo.value)),
  },
  {
    title: '負指數',
    formula: `${base.value}^(-${expTwo.value})`,
    value: formatNumber(1 / Math.pow(base.value, expTwo.value)),
  },
  {
    title: '零次方',
    formula: `${base.value}^0`,
    value: '1',
  },
  {
    title: '分數指數',
    formula: `${base.value}^(${expOne.value}/${root.value})`,
    value: formatNumber(Math.pow(base.value, expOne.value / root.value)),
  },
]);

const currentScenario = ref<Scenario>(buildScenario());

function flashHero() {
  heroAnimated.value = true;
  setTimeout(() => {
    heroAnimated.value = false;
  }, 1200);
}

function shuffleMeanings() {
  const decoys: MeaningItem[] = [
    { id: 'decoy1', text: '先把底數相乘後再平方，這是錯誤步驟' },
    { id: 'decoy2', text: '先做除法再決定指數是否存在，這是陷阱' },
  ];
  const pool = pairingCards.map((card, index) => ({
    id: card.id,
    text: `${index + 1}. ${scenarioTemplates[index]}`,
  }));
  meaningPool.value = [...pool, ...decoys].sort(() => Math.random() - 0.5);
}

function selectFormula(id: string) {
  activeFormula.value = id;
  feedbackMessage.value = '已選公式卡，點擊下方敘述卡完成配對。';
  feedbackType.value = 'neutral';
}

function chooseMeaning(id: string) {
  if (!activeFormula.value) {
    feedbackMessage.value = '請先選擇一張公式卡。';
    feedbackType.value = 'error';
    return;
  }

  if (matchedIds.value.has(activeFormula.value)) {
    feedbackMessage.value = '這張公式已完成配對，換下一張試試。';
    feedbackType.value = 'neutral';
    return;
  }

  if (id === activeFormula.value) {
    matchedIds.value.add(activeFormula.value);
    matchedIds.value = new Set(matchedIds.value);
    feedbackMessage.value = '配對成功！前往下一張卡片。';
    feedbackType.value = 'success';
    activeFormula.value = null;
  } else {
    feedbackMessage.value = '配對未命中，再想想對應的敘述。';
    feedbackType.value = 'error';
  }
}

function resetPairing() {
  matchedIds.value = new Set();
  activeFormula.value = null;
  feedbackMessage.value = '配對已重置，重新開始挑戰。';
  feedbackType.value = 'neutral';
  shuffleMeanings();
}

function rollDemoNumbers() {
  base.value = randomInRange(2, 9);
  expOne.value = randomInRange(1, 8);
  expTwo.value = randomInRange(1, 8);
  root.value = randomInRange(2, 6);
}

function randomInRange(min: number, max: number) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function formatNumber(value: number) {
  if (!isFinite(value)) return '未定義';
  const rounded = Math.round(value * 100) / 100;
  return Number.isInteger(rounded) ? rounded.toString() : rounded.toFixed(2);
}

function buildScenario(): Scenario {
  const cardIndex = pairingCards.length > 0 ? Math.floor(Math.random() * pairingCards.length) : -1;
  const card = cardIndex >= 0 ? pairingCards[cardIndex] : null;
  const baseVal = randomInRange(2, 7);
  const expA = randomInRange(2, 5);
  const expB = randomInRange(2, 5);
  if (!card) {
    return {
      title: '預備劇本',
      prompt: '目前沒有可用的公式卡，請稍後再試。',
      base: 1,
      expA: 1,
      expB: 1,
      rule: '暫無',
      answer: '暫無',
      strategy: '稍後再生成新的挑戰。',
    };
  }

  const prompt = scenarioTemplates[cardIndex] || '根據公式判斷最佳出招順序。';

  let answer = '';
  switch (card.id) {
    case 'multiply':
      answer = `${baseVal}^${expA} · ${baseVal}^${expB} = ${baseVal}^${expA + expB} = ${formatNumber(
        Math.pow(baseVal, expA + expB),
      )}`;
      break;
    case 'divide':
      answer = `${baseVal}^${expA} ÷ ${baseVal}^${expB} = ${baseVal}^${expA - expB} = ${formatNumber(
        Math.pow(baseVal, expA - expB),
      )}`;
      break;
    case 'power':
      answer = `(${baseVal}^${expA})^${expB} = ${baseVal}^${expA * expB} = ${formatNumber(
        Math.pow(baseVal, expA * expB),
      )}`;
      break;
    case 'product':
      answer = `(${baseVal}·2)^${expA} = ${baseVal}^${expA} · 2^${expA} = ${formatNumber(
        Math.pow(baseVal, expA) * Math.pow(2, expA),
      )}`;
      break;
    case 'quotient':
      answer = `(${baseVal}/2)^${expA} = ${baseVal}^${expA} ÷ 2^${expA} = ${formatNumber(
        Math.pow(baseVal, expA) / Math.pow(2, expA),
      )}`;
      break;
    case 'zero':
      answer = `${baseVal}^0 = 1；任何非零數的零次方都是 1。`;
      break;
    case 'negative':
      answer = `${baseVal}^(-${expA}) = 1/${baseVal}^${expA} = ${formatNumber(1 / Math.pow(baseVal, expA))}`;
      break;
    case 'fraction':
      answer = `${baseVal}^(${expA}/${expB}) = ${expB}√(${baseVal}^${expA}) = ${formatNumber(
        Math.pow(baseVal, expA / expB),
      )}`;
      break;
  }

  const strategy =
    card.id === 'multiply'
      ? '同底數相乘先保留底數，直接把指數相加。'
      : card.id === 'divide'
        ? '同底數相除變成指數相減，若變成負指數代表能量移到分母。'
        : card.id === 'power'
          ? '冪上再冪時把指數相乘，避免先乘底數造成誤差。'
          : card.id === 'product'
            ? '拆開括號內乘法，分別提升後再相乘，利於觀察可否約分或化簡。'
            : card.id === 'quotient'
              ? '商的冪要同時套用到分子分母，保持分母避免意外變小數。'
              : card.id === 'zero'
                ? '看到零次方直接標記為 1，防止誤以為結果為 0。'
                : card.id === 'negative'
                  ? '負指數代表倒數，先移到分母再與其他項目合併。'
                  : '分數指數分母是開根次數，分子是剩餘次方，可先開根再次方。';

  return {
    title: `${card.name} 劇本`,
    prompt: prompt || '根據公式判斷最佳出招順序。',
    base: baseVal,
    expA,
    expB,
    rule: card.name,
    answer,
    strategy,
  };
}

function generateScenario() {
  currentScenario.value = buildScenario();
  showAnswer.value = false;
}

shuffleMeanings();
</script>
