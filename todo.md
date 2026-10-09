# Лендинг-портфолио — план реализации

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Собрать одностраничный лендинг-портфолио цифрового мастера по `spec.md`: 7 секций, демо-агент, одна CTA, нативные scroll-анимации.

**Architecture:** Статический сайт на чистом HTML/CSS + минимальный vanilla JS. Токены и база отдельно от компонентов и слоя `animation-timeline`. Демо-агент — локальный сценарий без бэкенда; форма — UX + `mailto:`/endpoint на выбор.

**Tech Stack:** HTML5, CSS3 (`@supports`, `animation-timeline`, `svh`, `clamp`), vanilla JS, шрифты IBM Plex Mono + Bricolage Grotesque/Space Grotesk + Manrope.

**Spec:** `spec.md`

## Global Constraints

- Фон страницы: `#070707`; акцент только `#B5FF4D` на CTA, метках, фокусе.
- IBM Plex Mono — метки, навигация, метрики, номера шагов.
- CTA-текст везде: «Получить точную оценку»; на экране — одна акцентная кнопка.
- Анимации скролла — только CSS `animation-timeline`, с `@supports` и `prefers-reduced-motion`.
- Форма ≤ 3 полей; кейсы с `CONCEPT` и цифрой результата; цена от `$995`.
- Код пока не пишем, пока пользователь не утвердил `spec.md` и не выбрал способ исполнения плана.

## Review Focus

1. JS выключен → hero/кейсы/FAQ/форма читаемы; демо показывает статичный fallback.
2. `prefers-reduced-motion: reduce` → нет progress/reveal-анимаций, контент сразу виден.
3. Узкий мобильный (≈375px) → одна колонка, CTA не обрезана, демо usable.
4. Клавиатура → таб по якорям, демо-полю, FAQ `<details>`, полям формы; виден `:focus-visible`.
5. Длинный текст в кейсе/FAQ → сетка не ломается, сканируемость метка→название→результат сверху.

---

## Карта файлов

| Файл | Ответственность |
|---|---|
| `index.html` | Семантика 7 секций, шапка, прогресс-бар |
| `styles/tokens.css` | CSS-переменные из спеки |
| `styles/base.css` | Reset, типографика, контейнер, ритм 60/30/15 |
| `styles/components.css` | Header, hero, demo, proof, cases, services, pricing, FAQ, form, footer |
| `styles/animations.css` | `.progress`, `.reveal` + timeline |
| `scripts/demo-agent.js` | Подсказки, ответы, a11y-статус |
| `scripts/form.js` | Валидация UX, сообщение «24 часа» |
| `assets/` | Плейсхолдеры изображений кейсов |

---

### Task 1: Каркас страницы и дизайн-токены

**Files:**
- Create: `index.html`
- Create: `styles/tokens.css`
- Create: `styles/base.css`

**Interfaces:**
- Consumes: значения из `spec.md` §4
- Produces: подключенные токены `--bg`, `--accent`, `--font-mono`, контейнер `.container`

- [ ] **Step 1: Создать `styles/tokens.css` с палитрой и типографикой из спеки**

Точные значения: `--bg: #070707`, `--accent: #B5FF4D`, `--font-mono: "IBM Plex Mono", …`, ритм 60/30/15, `--h1`/`--h2` через `clamp`.

- [ ] **Step 2: Создать `styles/base.css` — фон, цвет текста, контейнер 1200–1280px, секции с отступом ~60px**

- [ ] **Step 3: Создать `index.html` с 7 пустыми `<section id="…">` по порядку из спеки + `<div class="progress" aria-hidden="true">`**

ID: `hero`, `proof`, `cases`, `services`, `pricing`, `faq`, `contact`.

- [ ] **Step 4: Проверка**

Открыть `index.html` в браузере.  
Expected: чёрный фон `#070707`, семь секций друг под другом, подключены шрифты (хотя бы mono).

- [ ] **Step 5: Commit** (только если пользователь попросил коммит)

```bash
git add index.html styles/tokens.css styles/base.css
git commit -m "chore: каркас лендинга и дизайн-токены"
```

---

### Task 2: Шапка и Hero (без живого демо)

**Files:**
- Modify: `index.html` (header + hero copy)
- Create: `styles/components.css` (header, hero, button)

**Interfaces:**
- Consumes: токены, `.container`
- Produces: `.site-header`, `.hero`, `.btn-accent`, якорные ссылки на секции

- [ ] **Step 1: Разметить шапку — бренд, 5 якорей, одна CTA на `#contact`**

Текст CTA: «Получить точную оценку».

- [ ] **Step 2: Разметить hero — заголовок, подзаголовок, слот `.hero__demo`, CTA из спеки §3.1**

Заголовок: «Собираю сайты, приложения и ИИ-агентов, которые снимают рутину с команды».

- [ ] **Step 3: Стили шапки и hero: `min-height: 85svh`, одна композиция, без карточек поверх слота демо**

- [ ] **Step 4: Проверка**

Expected: первый экран читается как одна композиция; бренд заметен; одна кислотная кнопка; якоря не конкурируют с CTA.

---

### Task 3: Демо ИИ-агента

**Files:**
- Modify: `index.html` (разметка демо + noscript fallback)
- Modify: `styles/components.css`
- Create: `scripts/demo-agent.js`

**Interfaces:**
- Consumes: контейнер `.hero__demo`
- Produces: `initDemoAgent(root: HTMLElement): void` — подсказки, ввод, ответы из словаря

- [ ] **Step 1: HTML региона демо — `role="region"`, label поля, 3–4 кнопки-подсказки, статус `DEMO · тестовые данные`, границы возможностей**

- [ ] **Step 2: `demo-agent.js` — словарь вопрос→ответ; без автоплея; Enter отправляет; подсказки заполняют диалог**

- [ ] **Step 3: Fallback в `<noscript>` или статичный стартовый диалог в HTML**

- [ ] **Step 4: Проверка**

Expected: клик по подсказке даёт осмысленный ответ; без JS виден статичный диалог и путь к `#cases`.

---

### Task 4: Proof + Кейсы

**Files:**
- Modify: `index.html`
- Modify: `styles/components.css`

**Interfaces:**
- Consumes: `.reveal` (подключить в Task 7, класс можно повесить заранее)
- Produces: блок `#proof` с 98% + методом; сетка до 6 кейсов

- [ ] **Step 1: Секция `#proof` — метрика 98% (mono, accent), текст метода про 200 запросов, стек одной строкой**

- [ ] **Step 2: Секция `#cases` — до 6 карточек по контенту `spec.md` §3.3, метка `CONCEPT` сверху, результат цифрой**

- [ ] **Step 3: Стили сканируемости — метка → название → результат до длинного абзаца**

- [ ] **Step 4: Проверка**

Expected: без картинки смысл карточки понятен; везде честный `CONCEPT`; 98% не «голая» цифра.

---

### Task 5: Услуги, процесс, цены

**Files:**
- Modify: `index.html`
- Modify: `styles/components.css`

**Interfaces:**
- Produces: `#services` (услуги + шаги 01–05), `#pricing` (от $995, состав, передача кода)

- [ ] **Step 1: 3–4 услуги текстом из спеки + таблица/список шагов 01–05**

- [ ] **Step 2: Цены — «от $995», пакет Старт, блок «от чего зависит смета», фраза про передачу кода заказчику**

- [ ] **Step 3: На экране цен — одна акцентная CTA «Получить точную оценку»**

- [ ] **Step 4: Проверка**

Expected: цена видна без скролла к форме; вендор-лок закрыт текстом про передачу кода.

---

### Task 6: FAQ, контакты, форма, подвал

**Files:**
- Modify: `index.html`
- Modify: `styles/components.css`
- Create: `scripts/form.js`

**Interfaces:**
- Produces: FAQ на `<details>`/`<summary>`; форма из 3 полей; прямой email/Telegram; `initContactForm(form: HTMLFormElement): void`

- [ ] **Step 1: 5 вопросов FAQ из спеки на `<details>`; стили `grid-template-rows: 0fr → 1fr`; плюс→крестик**

- [ ] **Step 2: Форма — Имя, Email/Telegram, Описание; кнопка той же формулировки; рядом прямые контакты-плейсхолдеры**

- [ ] **Step 3: `form.js` — базовая проверка заполненности; сообщение «Заявка принята. Отвечу в течение 24 часов.»**

- [ ] **Step 4: Подвал — имя, email, © год**

- [ ] **Step 5: Проверка**

Expected: FAQ открывается с клавиатуры; форма ≤3 полей; без формы остаётся email/Telegram.

---

### Task 7: Анимации `animation-timeline`

**Files:**
- Create: `styles/animations.css`
- Modify: `index.html` (подключить файл, классы `.reveal`)

**Interfaces:**
- Consumes: `.progress`, элементы с `.reveal`
- Produces: scroll progress + view reveal без JS

- [ ] **Step 1: Прогресс-бар — `@keyframes reading-progress`, внутри `@supports (animation-timeline: scroll())` и `prefers-reduced-motion: no-preference`; `animation-timeline` после `animation`**

- [ ] **Step 2: Reveal — базово `opacity: 1`; анимация `appear` только как progressive enhancement через `view()`**

- [ ] **Step 3: Навесить `.reveal` на кейсы, услуги, pricing-блоки (не на критичный hero-текст смысла)**

- [ ] **Step 4: Проверка**

Expected: при скролле растёт бар; при `prefers-reduced-motion` всё сразу видно; в браузере без timeline контент не скрыт.

---

### Task 8: Полировка a11y, мобильный, приёмка

**Files:**
- Modify: любые из выше по результатам проверки

**Interfaces:**
- Consumes: чек-лист приёмки `spec.md` §7 и Review Focus выше

- [ ] **Step 1: Пройти `spec.md` §7 пункт за пунктом и отметить в этом файле**

- [ ] **Step 2: Проверить Review Focus (1–5): noscript, reduced-motion, 375px, клавиатура, длинный контент**

- [ ] **Step 3: Заменить плейсхолдеры `[Имя]`, `[email@domain]`, `[@nickname]`, часовой пояс — когда пользователь даст данные**

- [ ] **Step 4: Финальный проход визуала — дисциплина акцента, нет второй CTA на экранах, нет автоплея**

---

## Чек-лист приёмки (дубль для галочек)

- [ ] Hero: выгода + ниша, 5–12 слов
- [ ] Демо агента работает, без автоплея, есть fallback
- [ ] 98% + метод (200 запросов)
- [ ] ≤6 кейсов, цифры, `CONCEPT`
- [ ] Процесс 5 шагов
- [ ] От $995, состав, передача кода
- [ ] FAQ на `<details>` (5 шт.)
- [ ] Форма ≤3 полей + прямой контакт + 24 часа
- [ ] Одна акцентная CTA / одна формулировка
- [ ] `#070707`, IBM Plex Mono, ритм ~60px
- [ ] Progress + reveal на `animation-timeline`
- [ ] `prefers-reduced-motion`, контраст, клавиатура
- [ ] Код ещё не начат до утверждения спеки (этот пункт снять после старта Task 1)

---

## Статус процесса Superpowers

| Этап | Статус |
|---|---|
| Brainstorming: понимание + ТЗ в `spec.md` | ✅ сделано — ждёт ревью |
| Writing-plans: план в `todo.md` | ✅ сделано — ждёт выбора способа исполнения |
| Реализация кода | ⏸ не начинать до утверждения |
