# Svetly Website Redesign — Design Spec

## Overview

Полная переработка svetly.shop — сайта-магазина христианского бренда Svetly (полиграфия для мам и детей). Один файл `index.html` с инлайн CSS и JS, деплой на GitHub Pages.

## Brand

- **Название:** Svetly (латиница). Логотип: Svet*ly* (курсивная часть «ly» терракотовым цветом)
- **Эстетика:** cottagecore, акварель, мягкие цвета. Тепло, свет, уют
- **Аудитория:** христианские мамы и семьи с детьми

## Color Palette

| Token | Hex | Usage |
|---|---|---|
| cream | #F7F2E8 | Основной фон |
| cream-dark | #EDE5D4 | Вторичный фон, trust pills, newsletter |
| parchment | #E8DCC8 | Границы, разделители |
| sage | #8A9E7A | Акцент зелёный |
| sage-light | #B5C4A8 | Декоративный зелёный |
| sage-dark | #5C7250 | Текст цитат |
| terra | #C4845A | Акцент терракотовый (цены, курсив) |
| dusty-rose | #D4A89A | Акцент розовый (About) |
| ink | #2C2018 | Основной текст, кнопки, навбар текст |
| ink-soft | #5C4A38 | Вторичный текст |
| deep-sage | #2A3328 | Фон About и Footer (вместо коричневого) |

## Typography

- **Заголовки:** Cormorant Garamond (Google Fonts) — 400, 500 weight, italic для акцентов
- **Основной текст:** Nunito — 300, 400, 500 weight
- **Цитаты:** Lora — italic

## Layout

- **Max-width:** 1200px для всех секций контента, центрировано
- **Mobile-first:** базовые стили для мобилы, `@media (min-width: 768px)` для десктопа
- **Один файл:** index.html с инлайн `<style>` и `<script>`

## Texture & Decoration

- **Текстура бумаги:** SVG noise filter на `body::before`, полупрозрачный
- **Ботанические SVG декоры:** ветки и листья в hero-секции, полупрозрачные (opacity 0.12-0.15)

## Sections

### 1. Navbar

**Desktop (768px+):**
- Логотип Svet*ly* слева (Cormorant Garamond, 26px)
- Горизонтальное меню: Каталог, О нас, Контакты
- Кнопка корзины справа: иконка 🛒 + бейдж-счётчик (тёмный фон, pill shape)

**Mobile:**
- Логотип + корзина + бургер-кнопка (3 полоски)
- Бургер открывает fullscreen меню (slide-in справа, cubic-bezier анимация)
- Меню: крупные ссылки (Cormorant Garamond, 32px), цитата из Писания внизу
- Кнопка закрытия ×

**Behavior:**
- Sticky: `position: fixed`, при скролле уплотняется (меньше padding), добавляется тень
- Backdrop-filter: blur(14px) для полупрозрачности

### 2. Hero

**Desktop:**
- Две колонки: `grid-template-columns: 1fr 1fr`
- **Слева:** бейдж «Христианская полиграфия» (pill shape) → заголовок (Cormorant Garamond, 52-68px) «Слово Божье в каждом *изделии*» → цитата с Пс. 118:105 (с border-left sage) → описание → два CTA (primary «Смотреть каталог» + ghost «О нас →»)
- **Справа:** фон cream-dark, сетка 2×2 из карточек товаров. Карточки: SVG/emoji illustration, название, цена. Сдвиг по вертикали (1-я и 4-я) для визуального ритма
- Ботанические SVG декоры позади контента

**Mobile:**
- Одна колонка: текст сверху (padding-top: 80px для навбара)
- Карточки товаров — горизонтальный скролл (flex, overflow-x: auto)
- Подсказка «листай →» под карточками

**Animation:**
- Элементы появляются последовательно: fadeUp с задержкой 0.1s между элементами

### 3. Trust Pills

- Полная ширина, фон cream-dark, border сверху и снизу
- 4 пилюли: 🌿 Сделано с молитвой | 📦 Доставка по России | ✦ Малый живой тираж | 💛 Для семьи и детей
- Desktop: по центру, flex-wrap
- Mobile: горизонтальный скролл

### 4. Categories

**Заголовок:** «Что мы *создаём*» + ссылка «Все категории»

**6 категорий:**
1. Постеры (библейские стихи) — gradient: #E0D8CC → #C4B498
2. Prayer Journals (планеры для мам) — gradient: #CDD9C4 → #A8C098
3. Раскраски (для детей по Библии) — gradient: #E4D0C8 → #C8A090
4. Наклейки (закладки, открытки) — gradient: #E8D8CC → #D4B8A0
5. Праздники (Пасха, Рождество) — gradient: #E0DCC8 → #C4B888
6. Activity Books (задания для детей) — gradient: #D4DCCC → #B0C0A0

Каждая карточка: gradient background, emoji иконка (opacity 0.4, по центру), название и подзаголовок внизу на gradient overlay (белый текст).

**Desktop:** grid 3×2, aspect-ratio: 4/5
**Mobile:** grid 2×3, чётные колонки сдвинуты вниз

**Hover (desktop):** scale(1.02), усиление тени

### 5. Scripture Quote

- Полная ширина, фон cream-dark, border сверху и снизу
- Декоративные символы ✦ ✦ ✦
- Цитата: Cormorant Garamond, italic, 28px (desktop 36px)
- «Филиппийцам 4:8» — uppercase, letter-spacing
- Max-width: 800px, по центру

### 6. Products

**Заголовок:** «Популярные *товары*» + ссылка «Все товары»

**6 товаров:**
1. Постер «Не бойся» А3 — 490₽ (тег: Постер)
2. Prayer Journal «Утро» — 620₽ (тег: Журнал)
3. Раскраска «Добрый пастырь» — 380₽ (тег: Детям)
4. Карточки «Обетования» 30 шт — 350₽ (тег: Карточки)
5. Наклейки «Вера» — 180₽ (тег: Наклейки)
6. Пасхальный набор для детей — 550₽ (тег: Праздники)

Карточка товара: gradient image area (aspect-ratio 1/1, emoji placeholder) → tag pill (sage) → название (Cormorant Garamond) → описание → цена (terra) + кнопка «В корзину»

**Desktop:** grid 3 колонки, 2 ряда (все 6 товаров видны)
**Mobile:** горизонтальный скролл, ширина карточки 230px

**Hover (desktop):** подъём тени, кнопка «В корзину» → background: terra
**Click «В корзину»:** текст меняется на «✓ Добавлено», фон → sage, через 1.8s возвращается

### 7. About

- **Фон:** deep-sage #2A3328 (полная ширина)
- **Контент:** max-width 1200px, по центру

**Desktop:** две колонки
- **Слева:** eyebrow «Наша история» (terra, uppercase) → заголовок «Сделано *с верой* в Краснодарском крае» (Cormorant Garamond, dusty-rose italic) → текст о семье из Белореченска
- **Справа:** сетка 2×2 карточек ценностей (✦ Вера, 🌿 Красота, 👨‍👩‍👧 Семья, 🤍 Качество) — стеклянный стиль (rgba фон, rgba border) → кнопка «Читать нашу историю» (outline, pill shape)

**Mobile:** одна колонка, текст → карточки → кнопка

### 8. Newsletter

- Фон cream-dark, border-top
- Декор ✦ ✦ ✦
- Заголовок: «Новинки *первыми*»
- Описание + форма (email input + кнопка «Подписаться»)
- **Desktop:** input и кнопка в одну строку (flex-direction: row)
- **Mobile:** стакаются вертикально
- Max-width: 480px, по центру

### 9. Footer

- **Фон:** deep-sage #2A3328

**Desktop:** 3 колонки (2fr 1fr 1fr)
- **Колонка 1:** логотип Svet*ly*, описание, соцсети (VK, TG, ✉)
- **Колонка 2:** Каталог — Постеры, Раскраски, Журналы, Наклейки
- **Колонка 3:** Магазин — Доставка, Оплата, О нас, Контакты

**Bottom bar:** border-top, копирайт + цитата

**Mobile:** одна колонка, ссылки в сетке 2 колонки

## Cart & Order System

### Cart (localStorage)

**Data structure:**
```js
// localStorage key: 'svetly-cart'
[{ id: 'poster-ne-boysya', name: '«Не бойся» А3', price: 490, qty: 1, tag: 'Постер' }, ...]
```

**Cart panel:**
- Slide-in панель справа (desktop: width 400px, mobile: fullscreen)
- Открывается по клику на корзину в навбаре или sticky кнопку
- Список товаров с ±кнопками количества, удаление
- Итого внизу + кнопка «Оформить заказ»
- Overlay затемнение за панелью

**Add to cart animation:**
- Кнопка «В корзину» → «✓ Добавлено» (фон sage) → через 1.8s обратно
- Бейдж в навбаре обновляется

**Mobile sticky cart:**
- Кнопка «🛒 Корзина · N» фиксирована внизу экрана
- Появляется когда в корзине есть товары (opacity + translateY анимация)
- Скрыта на десктопе

### Order Form

- Заменяет список товаров в той же панели
- Поля: Имя, Телефон/Email, Город, Комментарий
- Сводка заказа (товары + итого)
- Кнопка «Отправить заказ» → формирует текст заказа и открывает Telegram (`https://t.me/...?text=...`) с предзаполненным сообщением. Fallback: WhatsApp link
- Подпись: «Мы свяжемся с вами для подтверждения»

## Animations

- **fadeUp:** opacity 0→1, translateY 20px→0, 0.7s ease
- **Scroll reveal:** IntersectionObserver (threshold 0.08), каждая секция с классом `.reveal` получает `.visible`
- **Hero stagger:** элементы появляются с задержкой 0.1s
- **Nav transition:** padding и shadow меняются за 0.4s при скролле
- **Cart slide-in:** translateX(100%) → translateX(0), cubic-bezier(0.4, 0, 0.2, 1)
- **Hover (desktop only):** transform, box-shadow transitions 0.3s

## Technical

- **Один файл:** index.html (~1500-2000 строк)
- **Шрифты:** Google Fonts — Cormorant Garamond (300,400,500,600 + italic), Lora (400 italic), Nunito (300,400,500)
- **No frameworks:** чистый HTML + CSS + vanilla JS
- **CNAME:** svetly.shop (уже настроен)
- **Meta:** viewport, charset UTF-8, color-scheme: light only, lang="ru"
- **Deploy:** git push to main → GitHub Pages auto-deploy
