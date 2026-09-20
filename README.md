# StateSync — Telegram Mini App

Портал ролей / документів / анкет для RP-сервера Ukraine Horizon (Emergency Hamburg).

Це вже робочий фронтенд. Дані зберігаються в браузері (`localStorage`). Повний Roblox OAuth і спільна база для всіх гравців потребують бекенду — місце під це вже є в коді.

## Які файли і куди класти

Зроби папку, наприклад `statesync`, і поклади **точно таку структуру**:

```
statesync/
  index.html          ← головний файл, його відкриває Telegram
  css/
    styles.css
  js/
    i18n.js           ← українська + англійська
    app.js            ← уся логіка + CONFIG
  README.md
```

Нічого не перейменовуй. Mini App завжди стартує з `index.html`.

## Що змінити перед запуском

Відкрий `js/app.js` зверху, блок `CONFIG`:

| Поле | Що писати |
|---|---|
| `serverCode` | вже стоїть `6sez7maz` |
| `founder` | `@kitsuxen` |
| `createdBy` | `sqw1zyxx` |
| `siteUrl` | сайт сервера |
| `resourcesUrl` | пост з ресурсами |
| `adminPassword` | **обов’язково змініть** |
| `robloxClientId` | з Roblox Creator Hub (поки порожньо = демо) |
| `robloxRedirectUri` | `https://ТВІЙ-ДОМЕН/index.html` |

Поки `robloxClientId` порожній, кнопка «Прив’язати Roblox» просить лише публічний username. Пароль Roblox не береться.

## Як викласти в інтернет

Потрібен **https** домен. Найпростіше:

1. Залий папку `statesync` на GitHub.
2. GitHub Pages: Settings → Pages → Deploy from branch → root.
3. Отримаєш адресу на кшталт `https://username.github.io/statesync/`

Або Cloudflare Pages / Netlify: просто перетягни папку.

## Як підключити до Telegram-бота

1. У @BotFather створи бота або візьми існуючого.
2. Команда `/newapp` (або Bot Settings → Configure Mini App).
3. Title: `StateSync`
4. Web App URL: посилання на твій `index.html`
   приклад: `https://username.github.io/statesync/`
5. У боті зроби кнопку:

```
Меню бота → /setmenubutton
текст: StateSync
url: https://username.github.io/statesync/
```

Або в коді бота (aiogram / node-telegram-bot-api):

```
web_app: { url: "https://username.github.io/statesync/" }
```

## Прихована адмінка

На екрані політики 7 разів швидко натисни напис **STATESYNC**.
Відкриється пароль. За замовчуванням:

`ChangeMe_UH-RP_2026`

Зміни його в `CONFIG.adminPassword`.

В адмінці можна шукати гравця за Telegram/Roblox username або id (у демо — ті, хто вже відкривав цей самий браузер). Видача ролей собі працює одразу.

## Що вже є в інтерфейсі

- політика + галочка + підтвердити
- прив’язка Roblox / гість
- чорно-білий сплеш
- код сервера з копіюванням
- аватар справа → профіль
- меню: Головна, Ліцензії, Документи, Сервіси, Фракції, Профіль
- пошук документа за номером
- сервіси: сайт і t.me ресурси
- фракції: НПУ (КОРД, слідчі), Суд, Прокуратура + анкети
- статус Активний / Неактивний
- підпис пальцем
- штрафи, заявки, тема, вібрація, звук, мова UA/EN
- видалити акаунт

## Офіційний Roblox OAuth (пізніше)

1. https://create.roblox.com/docs/cloud/auth/oauth2-registration
2. Створи OAuth App, scopes: `openid` і `profile`.
3. Redirect URL = адреса Mini App.
4. Впиши `robloxClientId` і `robloxRedirectUri` у CONFIG.
5. Обмін `code` → токен треба робити на **сервері** (секрет не світити в Mini App).

Без бекенду гравці не мають спільної бази: у кожного свої дані в телефоні. Для реального сервера далі потрібен API (видача ролей усім, анкети адміну тощо).

## Перевірка без Telegram

Відкрий `index.html` у Chrome на телефоні або через Live Server. Telegram-аватар може бути порожнім — це нормально поза ботом.
