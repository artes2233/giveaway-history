# История розыгрышей — публичный сайт

Статическая страница: читает данные из Gist бота и показывает историю побед, рероллов и архив Nightbot.

## Что показывает

- счётчики: розыгрыши, победы/рероллы, уникальные победители, последний победитель, архив Nightbot;
- таблица розыгрышей: дата, победитель (аватар, ник, Twitch ID), приз, вывод, сайт, тип, источник;
- фильтры: поиск, тип, источник, диапазон дат, экспорт CSV;
- профиль по клику на ник: аватар, ID, победы/рероллы, статистика Nightbot, объединённая история;
- вкладка «Архив Nightbot» — 1529 записей из логов старого бота;
- кнопка «Проверить»: пересчёт `SHA256(randomness|salt|candidates_hash|final|shortlist_hash) mod K` прямо в браузере, сверка с записанным `final_index` и проверка, что победитель есть в шорт-листе;
- автообновление раз в минуту, кеш в localStorage при отсутствии сети.

## Источники данных

```
https://gist.githubusercontent.com/<owner>/<gistId>/raw/giveaway-history.json   — розыгрыши бота
https://gist.githubusercontent.com/<owner>/<gistId>/raw/nightbot-history.json   — архив Nightbot
```

CORS у Gist открыт (`*`), поэтому сервер не нужен.

## Настройки (в начале `<script>`)

```js
const GIST_OWNER = 'artes2233';
const GIST_ID    = '4ed64e61206c6ace4dbdfebff67a0ccc';
const CHANNEL    = 'nasuvan_tv';
const SHOW_ID    = true;   // показывать Twitch ID
const SHOW_PAYOUT = true;  // показывать сумму вывода
```

## Публикация на GitHub Pages (бесплатно)

1. Открыть https://github.com/new
2. Repository name: `giveaway-history` → **Public** → **Create repository**
3. На странице репозитория: **Add file → Upload files** → перетащить `index.html` → **Commit changes**
4. **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)` → **Save**
5. Через 1–2 минуты сайт будет доступен: `https://<ваш-логин>.github.io/giveaway-history/`

## Важно

- Репозиторий публичный, но в нём только `index.html` — никаких токенов и базы.
- Gist «secret»: ссылка на raw-файл видна в исходнике страницы, то есть данные становятся публичными (ники, ID, суммы). Если это нежелательно — уберите `SHOW_ID` / `SHOW_PAYOUT` или сделайте публичный gist с копией.
- Старые записи (до появления `shortlist`/`final_index`) проверяются частично: можно открыть раунд drand, но пересчитать победителя нельзя.
- QRNG и Random.org не сохраняли `salt` в старых записях, поэтому для них кнопка «Проверить» покажет «нет salt» — это нормально для истории.
