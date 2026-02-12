<!-- pages/index.vue -->
<template>
  <main class="page">
    <h1 class="title">Каталог</h1>

    <section class="section" aria-label="Эти товары часто покупают">
      <div class="sectionHead">
        <h2 class="sectionTitle">Эти товары часто покупают</h2>
      </div>

      <div class="rail" role="list">
        <template v-for="(p, idx) in products" :key="`${p.id}-${idx}`">
          <!-- 1 товар: открывает модалку + меняет URL на /product=1 (без перехода по роутеру) -->
          <button
              v-if="p.id === '1'"
              type="button"
              class="pCard"
              role="listitem"
              @click="openModalWithUrl(p.id)"
          >
            <div class="pMedia">
              <img class="pImg" :src="p.image" :alt="p.title" loading="lazy" />
              <span class="pFav" aria-label="В избранное" title="В избранное">♡</span>
            </div>

            <div class="pBody">
              <div class="pTitle">{{ p.title }}</div>

              <div class="pMetaRow">
                <span class="pMeta">≈ {{ p.etaMin }} мин</span>
                <span class="pPrice">{{ p.price }}</span>
              </div>

              <div class="pMetaRow pMetaRow2">
                <span class="pMeta">
                  <span class="pStar" aria-hidden="true">★</span>
                  {{ p.rating }} <span class="pMuted">{{ p.ratingCount }}</span>
                </span>
                <span class="pMeta">
                  <span class="pTruck" aria-hidden="true">🛵</span>
                  {{ p.deliveryPrice }}
                </span>
              </div>
            </div>
          </button>

          <!-- Остальные товары: обычный переход на страницу продукта -->
          <NuxtLink v-else :to="`/products/${p.id}`" class="pCard" role="listitem">
            <div class="pMedia">
              <img class="pImg" :src="p.image" :alt="p.title" loading="lazy" />
              <span class="pFav" aria-label="В избранное" title="В избранное">♡</span>
            </div>

            <div class="pBody">
              <div class="pTitle">{{ p.title }}</div>

              <div class="pMetaRow">
                <span class="pMeta">≈ {{ p.etaMin }} мин</span>
                <span class="pPrice">{{ p.price }}</span>
              </div>

              <div class="pMetaRow pMetaRow2">
                <span class="pMeta">
                  <span class="pStar" aria-hidden="true">★</span>
                  {{ p.rating }} <span class="pMuted">{{ p.ratingCount }}</span>
                </span>
                <span class="pMeta">
                  <span class="pTruck" aria-hidden="true">🛵</span>
                  {{ p.deliveryPrice }}
                </span>
              </div>
            </div>
          </NuxtLink>
        </template>
      </div>
    </section>

    <!-- Модалка -->
    <Teleport to="body">
      <div
          v-if="isModalOpen"
          class="modalOverlay"
          role="dialog"
          aria-modal="true"
          :aria-label="`Детали: ${activeProduct?.title ?? 'продукт'}`"
          @click.self="closeModalToRoot"
          @keydown.esc="closeModalToRoot"
          tabindex="-1"
      >
        <div class="modal">
          <div class="modalHeader">
            <h2 class="modalTitle">{{ activeProduct?.title }}</h2>
            <button
                type="button"
                class="modalClose"
                @click="closeModalToRoot"
                aria-label="Закрыть"
                title="Закрыть"
            >
              ✕
            </button>
          </div>

          <div v-if="activeProduct" class="modalBody">
            <img
                v-if="activeProduct.image"
                class="modalImg"
                :src="activeProduct.image"
                :alt="activeProduct.title"
                loading="lazy"
            />

            <div class="kvGrid">
              <p class="kv">
                <span class="kvLabel">ID</span>
                <span class="kvValue">{{ activeProduct.id }}</span>
              </p>

              <p class="kv">
                <span class="kvLabel">Цена</span>
                <span class="kvValue kvValueStrong">
                  <strong>{{ activeProduct.price }}</strong>
                </span>
              </p>
            </div>

            <div class="descBlock">
              <div class="descLabel">Описание</div>
              <p class="descText">{{ activeProduct.description }}</p>
            </div>
          </div>

          <div class="modalFooter">
            <button type="button" class="primaryBtn" @click="closeModalToRoot">Закрыть</button>
          </div>
        </div>
      </div>
    </Teleport>
  </main>
</template>

<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

type ProductCard = {
  id: string
  title: string
  price: string
  image: string
  etaMin: number
  rating: string
  ratingCount: number
  deliveryPrice: string
}

type ProductDetails = {
  id: string
  title: string
  description: string
  price: string
  image?: string
}

const products: ProductCard[] = [
  {
    id: '1',
    title: 'Букет из Манго',
    price: '29 000 ₸',
    image: '/products/1.jpg',
    etaMin: 104,
    rating: '4.86',
    ratingCount: 750,
    deliveryPrice: '2000 ₸'
  },
  {
    id: '2',
    title: 'Яркий букет',
    price: '24 900 ₸',
    image: '/products/2.jpg',
    etaMin: 85,
    rating: '4.92',
    ratingCount: 312,
    deliveryPrice: '1500 ₸'
  },
  {
    id: '3',
    title: 'Букет «Пастельное утро»',
    price: '24 900 ₸',
    image: '/products/2.jpg',
    etaMin: 85,
    rating: '4.92',
    ratingCount: 312,
    deliveryPrice: '1500 ₸'
  },
  {
    id: '4',
    title: 'Букет «Солнечный микс»',
    price: '24 900 ₸',
    image: '/products/2.jpg',
    etaMin: 85,
    rating: '4.92',
    ratingCount: 312,
    deliveryPrice: '1500 ₸'
  },
  {
    id: '5',
    title: 'Букет «Нежные розы»',
    price: '24 900 ₸',
    image: '/products/2.jpg',
    etaMin: 85,
    rating: '4.92',
    ratingCount: 312,
    deliveryPrice: '1500 ₸'
  },
  {
    id: '6',
    title: 'Букет «Вечерний сад»',
    price: '24 900 ₸',
    image: '/products/2.jpg',
    etaMin: 85,
    rating: '4.92',
    ratingCount: 312,
    deliveryPrice: '1500 ₸'
  }
]

const details: Record<string, ProductDetails> = {
  '1': {
    id: '1',
    title: 'Букет из Манго',
    description: 'Детальная информация в модальном окне (без перехода на страницу).',
    price: '29 000 ₸',
    image: '/products/1.jpg'
  },
  '2': {
    id: '2',
    title: 'Яркий букет',
    description: 'Детальная информация товара №2.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  }
}

const modalProductId = ref<string | null>(null)

const isModalOpen = computed(() => modalProductId.value === '1')
const activeProduct = computed(() => (modalProductId.value ? details[modalProductId.value] : null))

function parseModalIdFromPath(pathname: string): string | null {
  const prefix = '/product='
  if (!pathname.startsWith(prefix)) return null
  const id = pathname.slice(prefix.length).trim()
  return id || null
}

function syncModalFromLocation() {
  if (!import.meta.client) return
  modalProductId.value = parseModalIdFromPath(window.location.pathname)
}

function openModalWithUrl(id: string) {
  if (!import.meta.client) return
  if (id !== '1') return

  const nextPath = `/product=${id}`
  window.history.pushState({ modalProductId: id }, '', nextPath)
  modalProductId.value = id
}

function closeModalToRoot() {
  if (!import.meta.client) return

  window.history.pushState({ modalProductId: null }, '', '/')
  modalProductId.value = null
}

function onPopState() {
  syncModalFromLocation()
}

onMounted(() => {
  if (!import.meta.client) return
  syncModalFromLocation()
  window.addEventListener('popstate', onPopState)
})

onBeforeUnmount(() => {
  if (!import.meta.client) return
  window.removeEventListener('popstate', onPopState)
})
</script>

<style scoped>
.page {
  max-width: 1024px;
  margin: 0 auto;
  padding: 20px 12px 36px;
  font-family: Roboto, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif;
  color: #0f172a;
  background: #fff;
}

.title {
  margin: 12px 0 14px;
  font-size: 28px;
  line-height: 1.15;
  font-weight: 800;
  letter-spacing: -0.02em;
}

.section {
  margin-top: 12px;
}

.sectionHead {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 10px;
}

.sectionTitle {
  margin: 0;
  font-size: 18px;
  line-height: 1.25;
  font-weight: 800;
  letter-spacing: -0.01em;
}

.rail {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 12px;

  overflow: visible;
  padding: 2px 2px 10px;
}

.rail > * {
  scroll-snap-align: start;
}

/* Карточка */
.pCard {
  display: grid;
  grid-template-rows: auto 1fr;
  border-radius: 18px;
  background: #fff;
  text-decoration: none;
  color: inherit;

  border: 1px solid rgba(15, 23, 42, 0.1);
  box-shadow: 0 8px 24px rgba(15, 23, 42, 0.06);
  overflow: hidden;

  transition: transform 140ms ease, box-shadow 140ms ease, border-color 140ms ease;
  cursor: pointer;
}

.pCard:hover {
  transform: translateY(-2px);
  border-color: rgba(15, 23, 42, 0.16);
  box-shadow: 0 14px 36px rgba(15, 23, 42, 0.1);
}

.pCard:focus-visible {
  outline: 3px solid rgba(59, 130, 246, 0.45);
  outline-offset: 3px;
}

/* button reset */
button.pCard {
  appearance: none;
  padding: 0;
  border: 1px solid rgba(15, 23, 42, 0.1);
  text-align: left;
  background: #fff;
}

/* Медиа */
.pMedia {
  position: relative;
  aspect-ratio: 1 / 1;
  background: rgba(15, 23, 42, 0.04);
}

.pImg {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}

/* Избранное */
.pFav {
  position: absolute;
  top: 10px;
  right: 10px;

  width: 34px;
  height: 34px;
  border-radius: 999px;

  display: grid;
  place-items: center;

  background: rgba(255, 255, 255, 0.92);
  border: 1px solid rgba(15, 23, 42, 0.1);
  box-shadow: 0 6px 16px rgba(15, 23, 42, 0.12);

  font-size: 16px;
  line-height: 1;
  user-select: none;
}

/* Контент */
.pBody {
  padding: 10px 12px 12px;
  display: grid;
  gap: 8px;
}

.pTitle {
  font-size: 14px;
  line-height: 1.25;
  font-weight: 700;
  letter-spacing: -0.01em;

  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: calc(14px * 1.25 * 2);
}

.pMetaRow {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.pMetaRow2 {
  margin-top: -2px;
}

.pMeta {
  font-size: 12px;
  color: rgba(15, 23, 42, 0.7);
  display: inline-flex;
  align-items: center;
  gap: 6px;
  white-space: nowrap;
}

.pMuted {
  color: rgba(15, 23, 42, 0.45);
}

.pPrice {
  font-size: 13px;
  font-weight: 800;
  color: #0f172a;
  background: rgba(15, 23, 42, 0.04);
  padding: 6px 10px;
  border-radius: 999px;
  border: 1px solid rgba(15, 23, 42, 0.08);
  white-space: nowrap;
}

.pStar {
  color: #f59e0b;
}

.pTruck {
  filter: grayscale(1);
  opacity: 0.85;
}

/* Модалка */
.modalOverlay {
  position: fixed;
  inset: 0;
  background: rgba(2, 6, 23, 0.55);
  display: grid;
  place-items: center;
  padding: 16px;
  z-index: 1000;
}

.modal {
  width: min(720px, 100%);
  border-radius: 18px;
  background: #fff;
  border: 1px solid rgba(15, 23, 42, 0.1);
  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.25);
  overflow: hidden;
}

.modalHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 16px 12px;
  border-bottom: 1px solid rgba(15, 23, 42, 0.08);
}

.modalTitle {
  margin: 0;
  font-size: 18px;
  line-height: 1.2;
  font-weight: 900;
  letter-spacing: -0.01em;
}

.modalClose {
  border: 1px solid rgba(15, 23, 42, 0.1);
  background: rgba(15, 23, 42, 0.03);
  font-size: 18px;
  cursor: pointer;
  padding: 8px 10px;
  border-radius: 12px;
  line-height: 1;
}

.modalClose:hover {
  background: rgba(15, 23, 42, 0.06);
}

.modalBody {
  padding: 16px;
  display: grid;
  gap: 12px;
}

.modalImg {
  width: 100%;
  height: 280px;
  object-fit: cover;
  border-radius: 14px;
  border: 1px solid rgba(15, 23, 42, 0.1);
  background: rgba(15, 23, 42, 0.04);
  margin-bottom: 0;
}

/* ID + Цена (2 колонки) */
.kvGrid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.kv {
  margin: 0;
  border-radius: 14px;
  border: 1px solid rgba(15, 23, 42, 0.08);
  background: rgba(15, 23, 42, 0.02);
  padding: 10px 12px;
  display: grid;
  gap: 4px;
}

.kvLabel {
  font-size: 12px;
  letter-spacing: 0.01em;
  text-transform: uppercase;
  color: rgba(15, 23, 42, 0.55);
}

.kvValue {
  font-size: 14px;
  line-height: 1.35;
  color: rgba(15, 23, 42, 0.92);
  word-break: break-word;
}

.kvValueStrong {
  font-weight: 900;
}

/* Описание (лейбл сверху, текст ниже) */
.descBlock {
  border-radius: 14px;
  border: 1px solid rgba(15, 23, 42, 0.08);
  background: rgba(15, 23, 42, 0.015);
  padding: 12px;
  display: grid;
  gap: 6px;
}

.descLabel {
  font-size: 12px;
  letter-spacing: 0.01em;
  text-transform: uppercase;
  color: rgba(15, 23, 42, 0.55);
}

.descText {
  margin: 0;
  font-size: 14px;
  line-height: 1.6;
  color: rgba(15, 23, 42, 0.85);
  white-space: normal;
  word-break: break-word;
}

.modalFooter {
  padding: 12px 16px 16px;
  border-top: 1px solid rgba(15, 23, 42, 0.08);
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.primaryBtn {
  cursor: pointer;
  border: 1px solid rgba(15, 23, 42, 0.12);
  background: #0f172a;
  color: #fff;
  padding: 10px 14px;
  border-radius: 14px;
  font-weight: 800;
}

.primaryBtn:hover {
  background: #0f172a;
  filter: brightness(1.05);
}

@media (max-width: 680px) {
  /* Мобильная: карточки 100% ширины и идут вертикально */
  .rail {
    grid-auto-flow: row;
    grid-auto-columns: initial;
    grid-template-columns: 100%;

    overflow-x: hidden;
    overflow-y: visible;
    scroll-snap-type: none;

    padding: 2px 0 10px;
    justify-items: stretch;
  }

  .rail > * {
    width: 100%;
    scroll-snap-align: unset;
  }

  .pCard {
    width: 100%;
  }

  .modalOverlay {
    padding: 12px;
  }

  .modalHeader {
    padding: 14px 14px 10px;
  }

  .modalBody {
    padding: 14px;
    gap: 10px;
  }

  .modalImg {
    height: 220px;
  }

  .kvGrid {
    grid-template-columns: 1fr;
  }

  .modalFooter {
    padding: 10px 14px 14px;
  }

  .primaryBtn {
    width: 100%;
    justify-content: center;
  }
}
</style>