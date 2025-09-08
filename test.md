# Практика 4: CSS-архитектура: БЭМ, переменные, состояния
## Подробная пошаговая инструкция

### Шаг 1. Подготовка и подключение CSS

1. **Создайте единый файл стилей `styles.css`** в корне проекта
2. **Подключите его ко всем страницам** из Практик 2 и 3:

```html
<!-- В <head> каждой страницы -->
<link rel="stylesheet" href="styles.css">
```

### Шаг 2. Создание дизайн-токенов и reset

**Добавьте в начало `styles.css`:**

```css
/* ========================================
   ДИЗАЙН-ТОКЕНЫ
======================================== */
:root {
  /* Цветовая палитра */
  --color-bg: #fff;
  --color-fg: #0b0b0b;
  --color-muted: #666;
  --color-primary: #0a84ff;
  --color-primary-hover: #0066cc;
  --color-danger: #dc2626;
  --color-success: #16a34a;
  
  /* Типографика */
  --font-body: system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif;
  --font-mono: "SF Mono", Monaco, "Cascadia Code", "Roboto Mono", Consolas, monospace;
  
  /* Шкала отступов */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 16px;
  --space-4: 24px;
  --space-5: 40px;
  --space-6: 64px;
  
  /* Скругления и тени */
  --radius: 12px;
  --radius-small: 6px;
  --shadow: 0 1px 2px rgba(0,0,0,.08), 0 8px 24px rgba(0,0,0,.08);
  --shadow-strong: 0 4px 16px rgba(0,0,0,.12), 0 16px 48px rgba(0,0,0,.16);
  
  /* Размеры контейнеров */
  --container-max: 1120px;
  --content-max: 768px;
}

/* Тёмная тема */
body.theme-dark {
  --color-bg: #0b0b0b;
  --color-fg: #f5f5f5;
  --color-muted: #aaa;
  --color-primary: #6aa2ff;
  --color-primary-hover: #8bb4ff;
}

/* ========================================
   RESET И БАЗОВЫЕ СТИЛИ
======================================== */
* {
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
}

body {
  background: var(--color-bg);
  color: var(--color-fg);
  font-family: var(--font-body);
  line-height: 1.5;
  transition: background-color 0.2s, color 0.2s;
}

/* Плавная прокрутка к якорям */
html {
  scroll-behavior: smooth;
}

/* Медиа-элементы не выходят за границы */
img, video {
  max-width: 100%;
  height: auto;
  display: block;
}

/* Форм-контролы наследуют стили */
button, input, select, textarea {
  font: inherit;
  color: inherit;
}
```

### Шаг 3. БЭМ-структура для шапки и навигации

**Обновите HTML шапки (для всех страниц Пр2):**

```html
<header class="site-header">
  <div class="container">
    <a class="site-header__brand" href="index.html">
      <img src="img/logo.svg" alt="Логотип компании" class="site-header__logo">
      Наша Компания
    </a>
    
    <nav class="site-nav" aria-label="Основная навигация">
      <ul class="site-nav__list">
        <li class="site-nav__item">
          <a class="site-nav__link site-nav__link--current" href="index.html">Главная</a>
        </li>
        <li class="site-nav__item">
          <a class="site-nav__link" href="about.html">О нас</a>
        </li>
        <li class="site-nav__item">
          <a class="site-nav__link" href="contacts.html">Контакты</a>
        </li>
        <li class="site-nav__item">
          <a class="site-nav__link site-nav__link--cta" href="#contact-form">Связаться</a>
        </li>
      </ul>
    </nav>
  </div>
</header>
```

**CSS для шапки и навигации:**

```css
/* ========================================
   LAYOUT: КОНТЕЙНЕРЫ
======================================== */
.container {
  width: min(100% - 2 * var(--space-4), var(--container-max));
  margin-inline: auto;
}

.content {
  width: min(100% - 2 * var(--space-4), var(--content-max));
  margin-inline: auto;
}

/* ========================================
   БЛОК: ШАПКА САЙТА
======================================== */
.site-header {
  background: var(--color-bg);
  border-bottom: 1px solid rgba(0,0,0,0.1);
  box-shadow: var(--shadow);
  position: sticky;
  top: 0;
  z-index: 100;
}

.site-header .container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-block: var(--space-3);
}

.site-header__brand {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  text-decoration: none;
  color: var(--color-fg);
  font-weight: 600;
  font-size: 1.25rem;
}

.site-header__logo {
  width: 32px;
  height: 32px;
}

/* ========================================
   БЛОК: НАВИГАЦИЯ
======================================== */
.site-nav__list {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: var(--space-3);
}

.site-nav__link {
  display: block;
  padding: var(--space-2) var(--space-3);
  text-decoration: none;
  color: var(--color-muted);
  border-radius: var(--radius-small);
  transition: all 0.2s;
  font-weight: 500;
}

.site-nav__link:hover {
  color: var(--color-primary);
  background: rgba(10, 132, 255, 0.08);
}

.site-nav__link--current {
  color: var(--color-primary);
  background: rgba(10, 132, 255, 0.12);
}

.site-nav__link--cta {
  background: var(--color-primary);
  color: white;
}

.site-nav__link--cta:hover {
  background: var(--color-primary-hover);
  color: white;
}

/* Адаптивность навигации */
@media (max-width: 768px) {
  .site-header .container {
    flex-direction: column;
    gap: var(--space-3);
  }
  
  .site-nav__list {
    flex-wrap: wrap;
    justify-content: center;
  }
}
```

### Шаг 4. Хлебные крошки и якоря

**HTML для breadcrumbs:**

```html
<nav class="breadcrumbs" aria-label="Хлебные крошки">
  <ol class="breadcrumbs__list">
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="index.html">Главная</a>
    </li>
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="about.html">О нас</a>
    </li>
    <li class="breadcrumbs__item breadcrumbs__item--current" aria-current="page">
      Наша команда
    </li>
  </ol>
</nav>
```

**CSS для breadcrumbs и якорей:**

```css
/* ========================================
   БЛОК: ХЛЕБНЫЕ КРОШКИ
======================================== */
.breadcrumbs {
  padding: var(--space-3) 0;
  border-bottom: 1px solid rgba(0,0,0,0.1);
}

.breadcrumbs__list {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: var(--space-2);
}

.breadcrumbs__item:not(:last-child)::after {
  content: "→";
  margin-left: var(--space-2);
  color: var(--color-muted);
}

.breadcrumbs__link {
  color: var(--color-primary);
  text-decoration: none;
}

.breadcrumbs__link:hover {
  text-decoration: underline;
}

.breadcrumbs__item--current {
  color: var(--color-muted);
}

/* ========================================
   СОСТОЯНИЯ: ЯКОРЯ И ФОКУС
======================================== */
:target {
  outline: 2px dashed var(--color-primary);
  outline-offset: 4px;
  border-radius: var(--radius-small);
}

:where(a, button, input, select, textarea):focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}

:disabled,
[aria-disabled="true"] {
  opacity: 0.6;
  cursor: not-allowed;
}
```

### Шаг 5. Стилизация формы и модалки из Практики 3

**CSS для формы:**

```css
/* ========================================
   БЛОК: ФОРМА
======================================== */
.form {
  max-width: 500px;
  margin: 0 auto;
}

.form__field {
  margin-bottom: var(--space-4);
}

.form__label {
  display: block;
  margin-bottom: var(--space-2);
  font-weight: 600;
  color: var(--color-fg);
}

.form__input,
.form__select,
.form__textarea {
  width: 100%;
  padding: var(--space-3);
  border: 2px solid #d1d5db;
  border-radius: var(--radius-small);
  background: var(--color-bg);
  font-size: 1rem;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.form__input:focus,
.form__select:focus,
.form__textarea:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(10, 132, 255, 0.1);
}

.form__input[aria-invalid="true"],
.form__select[aria-invalid="true"],
.form__textarea[aria-invalid="true"] {
  border-color: var(--color-danger);
  background-color: rgba(220, 38, 38, 0.05);
}

.form__help {
  display: block;
  margin-top: var(--space-1);
  color: var(--color-muted);
  font-size: 0.875rem;
}

.form__actions {
  display: flex;
  gap: var(--space-3);
  margin-top: var(--space-5);
}

.form__button {
  flex: 1;
  padding: var(--space-3) var(--space-4);
  border: none;
  border-radius: var(--radius-small);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  cursor: pointer;
  transition: all 0.2s;
}

.form__button--primary {
  background: var(--color-primary);
  color: white;
}

.form__button--primary:hover {
  background: var(--color-primary-hover);
  transform: translateY(-1px);
}

.form__button--secondary {
  background: #6b7280;
  color: white;
}

.form__button--secondary:hover {
  background: #4b5563;
}

/* ========================================
   БЛОК: МОДАЛЬНОЕ ОКНО
======================================== */
.modal {
  padding: var(--space-5);
  border: none;
  border-radius: var(--radius);
  max-width: 500px;
  width: 90vw;
  box-shadow: var(--shadow-strong);
  background: var(--color-bg);
}

.modal::backdrop {
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
}

.modal__title {
  margin: 0 0 var(--space-4) 0;
  color: var(--color-fg);
  text-align: center;
  font-size: 1.5rem;
}

/* Анимация модалки */
.modal {
  opacity: 0;
  transform: scale(0.8);
  transition: opacity 0.2s, transform 0.2s;
}

.modal[open] {
  opacity: 1;
  transform: scale(1);
}
```

### Шаг 6. Работа с изображениями

**HTML для responsive изображений:**

```html
<!-- Responsive изображение с srcset -->
<img 
  class="card__image"
  src="img/project-640.jpg" 
  srcset="img/project-640.jpg 640w, img/project-960.jpg 960w, img/project-1280.jpg 1280w"
  sizes="(max-width: 700px) 92vw, (max-width: 1200px) 640px, 960px"
  alt="Проект: разработка мобильного приложения" 
  width="640" 
  height="360"
  loading="lazy">

<!-- Альтернативные форматы через picture -->
<picture class="hero__picture">
  <source type="image/avif" srcset="img/hero.avif">
  <source type="image/webp" srcset="img/hero.webp">
  <img 
    class="hero__image" 
    src="img/hero.jpg" 
    alt="Наша команда работает над проектом" 
    width="1920" 
    height="800">
</picture>
```

**CSS для изображений:**

```css
/* ========================================
   ЭЛЕМЕНТЫ: ИЗОБРАЖЕНИЯ
======================================== */
.card__image {
  width: 100%;
  height: 220px;
  object-fit: cover;
  object-position: center;
  border-radius: var(--radius-small);
  transition: transform 0.2s;
}

.card__image:hover {
  transform: scale(1.02);
}

/* ========================================
   БЛОК: ГЕРОЙ-СЕКЦИЯ
======================================== */
.hero {
  position: relative;
  background: 
    linear-gradient(to top, rgba(0,0,0,.6), rgba(0,0,0,.2)),
    url("img/hero-1920.jpg") center/cover no-repeat;
  color: white;
  padding: clamp(3rem, 8vw, 8rem) var(--space-4);
  border-radius: var(--radius);
  text-align: center;
  margin-bottom: var(--space-6);
}

.hero__title {
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  margin: 0 0 var(--space-3) 0;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.hero__subtitle {
  font-size: clamp(1.125rem, 2.5vw, 1.375rem);
  margin: 0 0 var(--space-5) 0;
  opacity: 0.9;
}

/* Адаптивный фон с image-set */
@supports (background-image: image-set(url() 1x)) {
  .hero {
    background-image:
      linear-gradient(to top, rgba(0,0,0,.6), rgba(0,0,0,.2)),
      image-set(
        url("img/hero-1280.jpg") 1x,
        url("img/hero-2560.jpg") 2x
      );
  }
}
```

### Шаг 7. Видео-контент

**HTML для видео:**

```html
<!-- Локальное видео -->
<div class="media">
  <video 
    class="media__video" 
    controls 
    preload="metadata" 
    playsinline
    width="960" 
    height="540" 
    poster="video/poster.jpg">
    <source src="video/demo-1080.mp4" type="video/mp4">
    <source src="video/demo-720.webm" type="video/webm">
    <track 
      kind="captions" 
      srclang="ru" 
      src="video/captions-ru.vtt" 
      label="Русские субтитры" 
      default>
    Ваш браузер не поддерживает видео.
  </video>
</div>

<!-- Встраиваемое видео -->
<div class="media">
  <iframe
    class="media__iframe"
    src="https://rutube.ru/play/embed/xxxxxxxxxx"
    title="Видео о нашей компании"
    allow="autoplay; encrypted-media; picture-in-picture; web-share"
    allowfullscreen 
    loading="lazy"></iframe>
</div>
```

**CSS для видео:**

```css
/* ========================================
   БЛОК: МЕДИА-КОНТЕЙНЕР
======================================== */
.media {
  aspect-ratio: 16/9;
  width: min(100%, 960px);
  margin: var(--space-5) auto;
  border-radius: var(--radius);
  overflow: hidden;
  box-shadow: var(--shadow);
}

.media__video,
.media__iframe {
  width: 100%;
  height: 100%;
  border: 0;
  display: block;
}

.media__video {
  background: #000;
}
```

### Шаг 8. Секции и контент

**CSS для контентных блоков:**

```css
/* ========================================
   БЛОКИ: СЕКЦИИ КОНТЕНТА
======================================== */
.section {
  padding-block: var(--space-5);
}

.section--alt {
  background: rgba(10, 132, 255, 0.03);
}

.section__title {
  font-size: clamp(1.75rem, 4vw, 2.5rem);
  margin: 0 0 var(--space-4) 0;
  text-align: center;
  color: var(--color-fg);
}

.section__content {
  max-width: var(--content-max);
  margin: 0 auto;
}

/* ========================================
   БЛОК: КАРТОЧКИ
======================================== */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-4);
  margin-top: var(--space-5);
}

.card {
  background: var(--color-bg);
  border-radius: var(--radius);
  padding: var(--space-4);
  box-shadow: var(--shadow);
  transition: transform 0.2s, box-shadow 0.2s;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-strong);
}

.card__title {
  font-size: 1.25rem;
  margin: var(--space-3) 0;
  color: var(--color-fg);
}

.card__text {
  color: var(--color-muted);
  line-height: 1.6;
}
```

### Шаг 9. Переключатель темы (дополнительно)

**HTML для переключателя:**

```html
<button class="theme-toggle" type="button" aria-pressed="false" title="Переключить тему">
  <span class="theme-toggle__icon">🌙</span>
  <span class="theme-toggle__text">Тёмная тема</span>
</button>
```

**CSS для переключателя:**

```css
/* ========================================
   БЛОК: ПЕРЕКЛЮЧАТЕЛЬ ТЕМЫ
======================================== */
.theme-toggle {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-3);
  background: var(--color-bg);
  border: 1px solid rgba(0,0,0,0.2);
  border-radius: var(--radius-small);
  cursor: pointer;
  transition: all 0.2s;
}

.theme-toggle:hover {
  background: var(--color-primary);
  color: white;
  border-color: var(--color-primary);
}

.theme-toggle__icon {
  font-size: 1.2em;
}

.theme-toggle__text {
  font-size: 0.875rem;
  font-weight: 500;
}

/* Состояние активной тёмной темы */
.theme-toggle[aria-pressed="true"] .theme-toggle__icon::before {
  content: "☀️";
}

.theme-toggle[aria-pressed="true"] .theme-toggle__text::before {
  content: "Светлая тема";
}
```

**JavaScript для переключателя:**

```javascript
// Добавить в main.js или отдельный файл theme.js
const themeToggle = document.querySelector('.theme-toggle');
const THEME_KEY = 'theme';

// Определение предпочтений пользователя
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
const savedTheme = localStorage.getItem(THEME_KEY);

// Инициализация темы
function initTheme() {
  const isDark = savedTheme === 'dark' || (!savedTheme && prefersDark);
  
  if (isDark) {
    document.body.classList.add('theme-dark');
    themeToggle?.setAttribute('aria-pressed', 'true');
  }
}

// Переключение темы
function toggleTheme() {
  const isDark = document.body.classList.toggle('theme-dark');
  themeToggle?.setAttribute('aria-pressed', String(isDark));
  localStorage.setItem(THEME_KEY, isDark ? 'dark' : 'light');
}

// Инициализация
initTheme();
themeToggle?.addEventListener('click', toggleTheme);
```

### Шаг 10. Финальная проверка и оптимизация

**Чек-лист перед сдачей:**

1. **Архитектура:**
   - [ ] styles.css подключен ко всем страницам
   - [ ] Все токены вынесены в `:root`
   - [ ] Нет использования `#id` для стилей
   - [ ] Нет `!important` и инлайн-стилей
   - [ ] Используется только шкала `--space-*`

2. **БЭМ:**
   - [ ] Все компоненты следуют БЭМ
   - [ ] Классы читаемы и логичны
   - [ ] Нет глубокой вложенности селекторов

3. **Доступность:**
   - [ ] `:focus-visible` работает корректно
   - [ ] `:target` подсвечивает якоря
   - [ ] Состояния `:hover`, `:disabled` реализованы
   - [ ] Контрастность цветов достаточная

4. **Медиа:**
   - [ ] Есть responsive изображение
   - [ ] Фон-секция с градиент-оверлеем
   - [ ] Видео корректно вписано в контейнер
   - [ ] `aspect-ratio` используется правильно

5. **Производительность:**
   - [ ] `loading="lazy"` для неважных изображений
   - [ ] `width` и `height` указаны у `<img>`
   - [ ] CSS сгруппирован по логическим блокам

### Структура итогового styles.css:

```css
/* ========================================
   ДИЗАЙН-ТОКЕНЫ
======================================== */
/* :root с переменными */

/* ========================================
   RESET И БАЗОВЫЕ СТИЛИ  
======================================== */
/* * { box-sizing } и базовые reset */

/* ========================================
   LAYOUT: КОНТЕЙНЕРЫ
======================================== */
/* .container, .content */

/* ========================================
   БЛОК: ШАПКА САЙТА
======================================== */
/* .site-header и элементы */

/* ========================================
   БЛОК: НАВИГАЦИЯ
======================================== */
/* .site-nav и элементы */

/* ========================================
   БЛОК: ХЛЕБНЫЕ КРОШКИ
======================================== */
/* .breadcrumbs и элементы */

/* ========================================
   БЛОК: ФОРМА
======================================== */
/* .form и элементы */

/* ========================================
   БЛОК: МОДАЛЬНОЕ ОКНО
======================================== */
/* .modal и элементы */

/* ========================================
   БЛОКИ: СЕКЦИИ КОНТЕНТА
======================================== */
/* .section, .card и другие блоки */

/* ========================================
   БЛОК: МЕДИА-КОНТЕЙНЕР
======================================== */
/* .media и элементы */

/* ========================================
   СОСТОЯНИЯ И УТИЛИТЫ
======================================== */
/* :target, :focus-visible, переключатель темы */

/* ========================================
   АДАПТИВНОСТЬ
======================================== */
/* @media запросы */
```

Эта инструкция покрывает все основные требования Практики 4 и поможет создать качественную