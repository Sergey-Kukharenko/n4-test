<!-- pages/products/[id].vue -->
<template>
  <main class="page">
    <header class="top">
      <NuxtLink class="back" to="/">← Назад в каталог</NuxtLink>
    </header>

    <section v-if="product" class="product">
      <div class="media">
        <img class="img" :src="product.image" :alt="product.title" loading="lazy" />
      </div>

      <div class="content">
        <h1 class="title">{{ product.title }}</h1>

        <div class="badges">
          <span class="badge">ID: {{ product.id }}</span>
          <span class="price">{{ product.price }}</span>
        </div>

        <p class="desc">{{ product.description }}</p>

        <div class="actions">
          <button type="button" class="primary">Добавить в корзину</button>
          <button type="button" class="ghost">В избранное</button>
        </div>

        <div class="details">
          <div class="detailCard">
            <div class="detailLabel">Доставка</div>
            <div class="detailValue">Сегодня / завтра</div>
          </div>
          <div class="detailCard">
            <div class="detailLabel">Упаковка</div>
            <div class="detailValue">Подарочная лента</div>
          </div>
          <div class="detailCard">
            <div class="detailLabel">Открытка</div>
            <div class="detailValue">В подарок</div>
          </div>
        </div>
      </div>
    </section>

    <section v-else class="notFound">
      <h1 class="title">Товар не найден</h1>
      <p class="muted">Проверьте ссылку или вернитесь в каталог.</p>
      <NuxtLink class="back backBig" to="/">← В каталог</NuxtLink>
    </section>
  </main>
</template>

<script setup lang="ts">
const route = useRoute()

type Product = {
  id: string
  title: string
  description: string
  price: string
  image: string
}

const catalog: Record<string, Product> = {
  '1': {
    id: '1',
    title: 'Букет из Манго',
    description: 'Экзотическая композиция с солнечным настроением: яркая, сочная и очень фотогеничная.',
    price: '29 000 ₸',
    image: '/products/1.jpg'
  },
  '2': {
    id: '2',
    title: 'Яркий букет',
    description: 'Смелый микс сезонных цветов — когда хочется “вау” без лишних слов.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  },
  '3': {
    id: '3',
    title: 'Букет «Пастельное утро»',
    description: 'Нежные оттенки и мягкие фактуры — идеальный вариант для уютного, тёплого повода.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  },
  '4': {
    id: '4',
    title: 'Букет «Солнечный микс»',
    description: 'Лёгкая, жизнерадостная композиция, которая добавляет света даже в пасмурный день.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  },
  '5': {
    id: '5',
    title: 'Букет «Нежные розы»',
    description: 'Классика в современной подаче: аккуратно, элегантно и всегда уместно.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  },
  '6': {
    id: '6',
    title: 'Букет «Вечерний сад»',
    description: 'Глубокие оттенки и контраст — выразительный букет, который выглядит “дорого”.',
    price: '24 900 ₸',
    image: '/products/2.jpg'
  }
}

const id = computed(() => String(route.params.id ?? ''))
const product = computed(() => catalog[id.value])
</script>

<style scoped>
.page {
  max-width: 1024px;
  margin: 0 auto;
  padding: 22px 12px 40px;
  font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
  color: #0f172a;
}

.top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
}

.back {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: inherit;
  opacity: 0.78;
  text-decoration: none;
  padding: 8px 10px;
  border-radius: 12px;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: rgba(15, 23, 42, 0.02);
}
.back:hover {
  opacity: 1;
  background: rgba(15, 23, 42, 0.04);
}
.backBig {
  margin-top: 10px;
}

.product {
  display: grid;
  grid-template-columns: 1.05fr 0.95fr;
  gap: 18px;
  align-items: start;
}

.media {
  border-radius: 18px;
  overflow: hidden;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: rgba(15, 23, 42, 0.03);
  box-shadow: 0 18px 50px rgba(15, 23, 42, 0.10);
}

.img {
  width: 100%;
  height: clamp(260px, 45vw, 520px);
  object-fit: cover;
  display: block;
}

.content {
  border-radius: 18px;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: #fff;
  box-shadow: 0 18px 50px rgba(15, 23, 42, 0.08);
  padding: 16px;
}

.title {
  margin: 0 0 10px;
  font-size: 28px;
  line-height: 1.1;
  font-weight: 900;
  letter-spacing: -0.02em;
}

.badges {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  align-items: center;
  margin-bottom: 12px;
}

.badge {
  font-size: 12px;
  color: rgba(15, 23, 42, 0.72);
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: rgba(15, 23, 42, 0.03);
  padding: 6px 10px;
  border-radius: 999px;
}

.price {
  font-size: 14px;
  font-weight: 900;
  color: #0f172a;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: rgba(15, 23, 42, 0.05);
  padding: 7px 12px;
  border-radius: 999px;
}

.desc {
  margin: 0 0 14px;
  font-size: 14px;
  line-height: 1.55;
  color: rgba(15, 23, 42, 0.82);
}

.actions {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin-bottom: 14px;
}

.primary,
.ghost {
  cursor: pointer;
  border-radius: 14px;
  padding: 12px 12px;
  font-weight: 800;
  border: 1px solid rgba(15, 23, 42, 0.12);
}

.primary {
  background: #0f172a;
  color: #fff;
  border-color: rgba(15, 23, 42, 0.85);
}
.primary:hover {
  filter: brightness(1.05);
}

.ghost {
  background: rgba(15, 23, 42, 0.03);
  color: #0f172a;
}
.ghost:hover {
  background: rgba(15, 23, 42, 0.06);
}

.details {
  display: grid;
  grid-template-columns: 1fr;
  gap: 10px;
}

.detailCard {
  border-radius: 14px;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: rgba(15, 23, 42, 0.02);
  padding: 10px 12px;
}

.detailLabel {
  font-size: 12px;
  color: rgba(15, 23, 42, 0.65);
  margin-bottom: 2px;
}

.detailValue {
  font-size: 13px;
  font-weight: 800;
  color: rgba(15, 23, 42, 0.90);
}

.notFound {
  border-radius: 18px;
  border: 1px solid rgba(15, 23, 42, 0.10);
  background: #fff;
  padding: 18px;
  box-shadow: 0 18px 50px rgba(15, 23, 42, 0.08);
}

.muted {
  margin: 6px 0 0;
  color: rgba(15, 23, 42, 0.65);
}

@media (max-width: 860px) {
  .product {
    grid-template-columns: 1fr;
  }
  .actions {
    grid-template-columns: 1fr;
  }
}
</style>