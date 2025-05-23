# Проект: CAPITALIST (B2B, FinTech) — https://capitalist.net

## Описание

Capitalist — это B2B-финтех-платформа, предоставляющая компаниям и частным лицам инструменты для массовых выплат, работы с электронными кошельками, валютными счетами и решения задач международных переводов.

Проект ощущается как серьёзная платформа, где важны безопасность, надёжность и высокая отказоустойчивость. Скорее не про визуальные изыски, а про стабильную инфраструктуру и богатый бэк.

---

## Кто пользуется

-   **Финансовые менеджеры** — управляют выплатами, курсами, отчетами.
-   **Партнёрские сервисы** — интегрируются через API и получают выплаты автоматически.
-   **Индивидуальные пользователи** — получают выплаты, выводят деньги, проверяют статус транзакций.
-   **Тех. команды клиентов** — подключают системы автоматизации с помощью API Capitalist.

---

## Бросилось в глаза

-   Используется **Next.js**, причём актуальной версии — 14.2.26, что говорит о хорошем уровне сопровождения.
-   В проекте реально дофига всего намешано: и **styled-components**, и **TailwindCSS**, и **Bootstrap**, и **CSS-модули**. Такое бывает, когда дизайн-систему не закрепили в самом начале, или проект долго эволюционировал.
-   Помимо **React**, в системе засветился и **Vue 2.7.14** — возможно, используется для отдельных виджетов или страниц.
-   Удивительно большое количество библиотек: `core-js`, `modernizr`, `lodash`, `jQuery`. Видно, что есть легаси и слой backward-совместимости.
-   Используется **Cloudflare CDN**, а также HTTP/3 + PWA — упор на быструю доставку, оффлайн и стабильность.
-   В Network до сих пор не отображаются JS-бандлы — вероятно, активно используется кеширование и preloading через `next/static` + headers.

---

## Структура продукта по блокам

-   Кабинет клиента (главный дэшборд, балансы, отчёты)
-   Модуль массовых выплат
-   Генерация ссылок/счетов на оплату
-   Сервис транзакций и статусы операций
-   Интеграции с внешними API
-   Профиль пользователя / безопасность
-   Система аналитики и логи
-   Поддержка (тикеты, обратная связь)

---

## Примерный стек

-   **Frontend**: Next.js + React + Tailwind + Bootstrap + styled-components (да, всё вместе). Местами используются CSS Modules.
-   **State management**: Redux.
-   **Сервер**: Java (видно по стеку).
-   **CDN**: Cloudflare.
-   **Библиотеки**: Lodash, jQuery, Modernizr, core-js (легаси-основа).
-   **Сборка**: Webpack, PWA, Priority Hints, HTTP/3.
-   **Аналитика**: Google Analytics, Yandex Metrika, GTM.
-   **Security**: HSTS, кеширование, защита через Cloudflare.

На публичной части сайта WebSocket-соединения не зафиксированы.  
Вероятнее всего, WebSocket не используется в клиентской части или доступен только в закрытой админ-панели.

---

## Где могла бы быть боль у фронта

-   **Поддержка гибридного UI** (Tailwind + Bootstrap + styled-components) — это как одновременное общение на трёх языках.
-   **Легаси-библиотеки** (jQuery, lodash, modernizr) мешают унификации UI и обновлению стека.
-   **Финансовые операции** = высокая цена ошибки. Все действия должны быть с валидацией, повторной проверкой, подтверждением.
-   **Множественные состояния** (например, "транзакция ожидает", "в обработке", "ошибка перевода") — сложная логика на фронте, особенно при real-time обновлениях.
-   **Безопасность** — максимальное внимание к XSS, CSRF, session hijacking. Всё должно быть закрыто наглухо.
-   **Работа с PDF/экспортом** — скорее всего, есть задачи выгрузки документов, а это отдельный ад.

---

## Интересные детали, которые хочется пощупать

-   Как реализованы компоненты UI при таком винегрете стилей?
-   Как обрабатываются ошибки при трансферах и как UI на них реагирует?
-   Используется ли real-time обновление статусов транзакций (через polling или WebSocket)?
-   Где граница между публичной и авторизованной частью?
-   Как работает масштабирование внутри Next.js (ISR, SSR, CSR)?
-   Как кешируются дэшборды? Есть ли SWR или custom hooks?

---

## Что стоит пощупать руками

### Кабинет пользователя

-   Разделение по ролям (что видит клиент, что — админ)
-   Отчёты и аналитика
-   Навигация по вкладкам, фильтры

### Модуль выплат

-   Ввод реквизитов
-   Массовая загрузка CSV/Excel
-   Проверка перед отправкой
-   UI ошибок

### Интеграции и API

-   Что видно о статусе интеграции
-   Есть ли логирование
-   Возможность тестировать API прямо из UI

### Безопасность

-   МФА
-   Лог действий
-   IP-ограничения, whitelist

---
