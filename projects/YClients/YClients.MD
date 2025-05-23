# Проект: YCLIENTS(B2B,SaaS) (https://www.yclients.com/?ab_redir=1743603460513)

## Описание проекта (что это такое?)

YCLIENTS — это CRM-система, которая помогает бизнесам в сфере услуг (салоны, студии, клиники и пр.) автоматизировать запись клиентов, управлять расписанием персонала, вести финучёт, отправлять напоминания и собирать аналитику.

Она ощущается не просто как CRM, а как рабочий инструмент "на каждый день" — что-то вроде расширенной админки для офиса, в которой и план, и деньги, и связи с клиентами.

Запись клиентов — это центральный блок продукта. Если что-то пойдёт не так на этом этапе, бизнес потеряет деньги напрямую: клиент не записался, не пришёл, забыли о визите, забыли напомнить — это всё сразу отражается на доходе. Поэтому именно запись с жёсткой привязкой ко времени — это точка давления и зона максимального внимания.

---

## Кто и как им пользуется

-   Администраторы: ведут записи, управляют сменами, получают звонки.
-   Мастера / сотрудники: смотрят свои загрузки, приходят по графику, иногда сами корректируют.
-   Владельцы: видят общую картину, финансовую модель, эффективность.
-   Клиенты: записываются через встроенный на сайт виджет (выбор мастера, услуги, даты).

Разные роли решают в системе разные задачи, и видно, что интерфейсы для них проектировались по-разному.

Конфликты в расписании — это ответственность и системы, и человека. Скорее всего, система должна предупреждать, блокировать дубль, но бывают ситуации, когда админ решает “впихнуть” запись вручную — и тут возможен оверрайд. Я бы сделал предупреждение и запрет по умолчанию, но с возможностью ручного подтверждения.

Что касается фронтенда — мне кажется, интерфейсы для клиентов (виджет), для админов (журнал) и для мастеров (личный график) сделаны раздельными. Возможно, это один проект, но с разными роутами и конфигами. А возможно — физически разные SPA, особенно виджет, который встраивается на сайт.

---

## Что мне сразу бросилось в глаза

Интерфейс перегружен, но в хорошем смысле — плотный, как cockpit у пилота. Там всё рядом, всё кликается, двигается, обновляется. Видно, что продукт зрелый, с кучей накопленных фичей и пользовательского опыта.

Drag & drop в расписании — судя по плавности и отзывчивости — скорее всего кастомный, не стандартная библиотека. Возможно, используется transform + mouse events, а не React DnD.

Виджет записи визуально отстранён от остальной системы. Скорее всего, это независимое мини-приложение, встраиваемое в сайты клиентов. Вероятно, в виде iframe или через script, как виджет Яндекс.Карт.

Когда открываешь расписание с десятками мастеров — скорее всего, колонки не рендерятся сразу. Логичнее всего, что используется windowing или lazy rendering по скроллу. Иначе слишком большая нагрузка на DOM. Возможно, своя реализация или что-то вроде `react-window`.

## Также видно, что публичный сайт собран через Tilda или подобный конструктор. Классы вида `t-rec`, `t-container` и т.д. это типичный паттерн Tilda. Это подтверждает мысль о том, что основная разработка ведётся не на этом сайте, а внутри ЛК, куда ведёт авторизация.

## Структура продукта по блокам

-   Расписание (электронный журнал)
-   Виджет онлайн-записи (внешняя точка входа)
-   Клиентская база (история, предпочтения, контакт)
-   Финансовый модуль (доходы, расходы, ЗП)
-   Склад / продажи (учёт товаров, инвентаризация)
-   Программы лояльности (бонусы, акции)
-   Уведомления (SMS, email, пуши)
-   Дашборды и отчёты (аналитика)
-   Профили мастеров (график, эффективность, рейтинг)

Скорее всего, каждый блок — это не отдельный SPA, а часть одного большого проекта, где модули лежат за роутами, и используются динамические импорты. Хотя виджет, как и говорил — может быть вообще отдельно.

---

## Примерный стек

Frontend, скорее всего, React с TypeScript. Есть ощущение, что компоненты типизированы. Возможно, часть старого интерфейса ещё на Vue — редко встречаются такие гибриды, но в больших продуктах это не редкость.

State — скорее всего Redux, потому что чувствуется, что состояние сложное и распределённое. Но может быть и MobX, если проект более старый.

CSS — похоже, кастомный, местами с SASS или BEM, возможно есть микс с CSS-in-JS на новых экранах.

API — судя по поведению, REST. Хотя в теории могли бы использовать GraphQL, но по скорости и структуре больше похоже на обычный API с чанками.

UI-kit точно не сторонний. Либо своя дизайн-система, либо чистый кастом.

Вероятно, используются GSAP-анимации на промо-страницах и, возможно, в клиентской части для микровзаимодействий.

На публичной части проекта используется jQuery (возможно, из Tilda),скорее всего легаси на проекте

Логику подтверждения записи я бы оставил на бэке. Виджет может предлагать слоты, но окончательное бронирование должно быть через API, чтобы избежать конфликтов.

Если виджет вставляется на сайт, логичнее было бы сделать его через iframe или как SPA с shadow DOM, чтобы не конфликтовал со стилями основного сайта.

Также видно, что сервер отдаёт заголовки `Cache-Control` и `ETag`, что говорит о продуманной системе кеширования на уровне браузера. Это особенно важно для повторных визитов клиентов или админов — страницы быстрее открываются, уменьшается нагрузка на сервер.

Публичный сайт собран через Tilda — это видно по подключённым скриптам (`tilda-animation`, `tilda-popup`, `tilda-lazyload`) и характерным классам (`t-rec`, `t-container`). Видимо, это визитка для SEO и привлечения, а сам продукт — за авторизацией.

На публичной части сайта WebSocket-соединения не зафиксированы.  
Вероятнее всего, WebSocket не используется в клиентской части или доступен только в закрытой админ-панели.

Трекинговых скриптов очень много: Google Analytics, GTM, Yandex Metrika, Roistat, VK Retargeting, что говорит о том, что маркетинг и аналитика — значимая часть бизнеса. На фронте это даёт дополнительные вызовы — все эти скрипты могут конфликтовать с твоими фичами (задержки рендера, влияние на производительность и т.п.).

## Где могла бы быть боль у фронта

Слоты — это адовая зона. Там могут быть слоты разной длительности, с перетаскиванием, зумом, навигацией по дням, пересечениями и так далее. Особенно больно — в мобилке.

Drag & drop — ещё один источник боли. Веб не любит сложные перетаскивания, особенно на тачах.

Фильтрация и состояние расписания — очень много переменных: выбранная дата, мастер, услуга, загруженность, фильтры. Держать это всё в одном месте тяжело.

Если несколько админов работают одновременно — возможны гонки данных. Например, один меняет слот, второй его уже заполнил, кто победит? Без системы блокировок и валидаторов — будет бардак.

Тестировать такие интерфейсы тоже непросто. Drag & drop руками тестить больно. Автотестами — дорого и нестабильно. Уведомления — вообще отдельный сценарий, там сложно воспроизвести баги.

---

## Интересные детали, которые хочется пощупать

-   Как реализован drag&drop: через transform или absolute?
-   Как они решают проблему sticky-заголовков и боковых панелей? Не scroll-sync ли?
-   Как виджет подгружает данные? Есть ли prefetch, как обрабатывается медленный интернет?
-   Что происходит при отмене действия — есть ли rollback?
-   Как решается вопрос адаптивности в таблицах?
-   Есть ли кеш расписания, и как долго он живёт?
-   Мастеров может быть 50+. Как рендерить их без фризов?
-   Как они обрабатывают старые и новые форматы запросов? Есть ли единая прослойка?
-   Как внутри сочетается Tilda и кастомная разработка? Где проходит граница?
-   Где именно хранится логика авторизации и как она делится между виджетом и основной системой?

---

## Что стоит пощупать руками

### Журнал записей (расписание)

-   Перетаскивание (drag & drop) записей
-   Реакция интерфейса при изменении времени
-   Что происходит при конфликте слотов
-   Как реализована отмена/удаление

### Виджет онлайн-записи

-   Как устроен флоу записи (шаги, валидация)
-   Как виджет встраивается на сторонние сайты
-   Есть ли адаптация под мобильные

### Уведомления

-   Какие события триггерят отправку
-   Как отображаются статусы доставки
-   Что происходит при ошибках

### Фильтрация и поиск

-   Как устроены фильтры в клиентской базе и расписании
-   Есть ли сохранение фильтров при навигации
-   Можно ли искать по части имени/номера

### Мультипользовательский режим

-   Что происходит при одновременном редактировании
-   Есть ли система блокировки слотов
-   Как быстро обновляется расписание при действиях другого пользователя

### Интеграции

-   Как выглядят настройки интеграций (кассы, CRM, SMS)
-   Есть ли статус-система для отправок
-   Как видна история ошибок или отправок

### Страница мастера

-   Какие метрики отображаются
-   Как отображается эффективность
-   Можно ли фильтровать по времени/услугам

---
