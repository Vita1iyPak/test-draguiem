# Draugiem Platform Setup
⏰ Estimated Time: ~2–3 hours (excluding waiting time for Olafs approval and Latvian translation review)

По всему тексту встречаются плейсхолдеры — замените их на значения своей игры:

| Плейсхолдер | Что подставить | Пример (Castle Craft) |
|---|---|---|
| `[GAME_TITLE]` | Публичное название игры | Castle Craft |
| `[GAME_SHORT_NAME]` | Internal slug (латиница, lowercase) | hustlemerge |
| `[APP_ID]` | ID приложения на frype.com | 15021196 |
| `[APP_URL]` | URL продакшн-сборки HTML5 | `hustlemerge.cleverappssg.com/publish/html5/` |
| `[GOOGLE_PLAY_URL]` | Ссылка на Google Play | `play.google.com/store/apps/details?id=com.cleverapps.hustlemerge` |
| `[APP_STORE_URL]` | Ссылка на App Store (обязательно `itunes.apple.com/lv/...`) | `itunes.apple.com/lv/app/id6670314553` |
| `[APP_KEY]` | API key из дашборда приложения | (из дашборда frype.com) |
| `[CI_BOT]` | Telegram CI-бот для вашей игры | `@cleverapps_stands_bot` (для merge-игр) |

---

## 1. Prerequisites

Copy this checklist and rename it for a specific game. Once all of the steps from Draugiem Tab are completed, attach the spreadsheet to the task and proceed with the integration.

[Checklist](https://docs.google.com/spreadsheets/d/1_PlNiliAx0hxyWPyZv-UYQal89YJUJnb9OzYrWuwwGY/edit?gid=1527026538#gid=1527026538)

Что должно быть сделано до начала:

- [ ] **Account access** — есть логин/пароль от аккаунта менеджера на frype.com (хранится в чеклисте)
- [ ] **iap enabled** — Paid Services включены для приложения (активирует Olafs)
- [ ] **share localizations** — таблица с латышскими переводами подготовлена и отправлена Olafs на ревью
- [ ] **list of promos** — файл `draugiemproducts.js` добавлен в репо (`cleverapps/src/[GAME_SHORT_NAME]/draugiemproducts.js`) — попросите разработчика
- [ ] **Assets** — все промо-материалы для Draugiem готовы (см. шаг 5). ⚠️ `banner_1920x1080` должен быть в формате `.png`, не `.jpg`. Если у вас `.jpg` — попросите художников из Launch team (`@anton_prokudiin`) предоставить `.png`.

---

## 2. Create app on Draugiem (Manager)

> ℹ️ Платформа доступна по двум доменам: **draugiem.lv** и **frype.com** — это одно и то же. Используйте **frype.com** (на нём английский интерфейс).

2.1 Go to https://www.frype.com/applications/dev/myapps/ and click **Create Integrated Application**.

2.2 Fill in the form:

- Title: `[GAME_TITLE]`
- Short name: `[GAME_SHORT_NAME]` (только латиница, lowercase)
- URL: `https://[APP_URL]`
- Description: (short game description)
- Developer email: `cleverappssg@gmail.com`
- Contact phone: `+971508263685`

2.3 Save the form. You will get an **App ID** and **API Key** — copy them for the next step.

🖼️ Result example: https://gyazo.com/98a1c0df0168c24b93235c2a938ea71d

---

## 3. Update configs (Manager + Developer)

3.1 Manager fills in the configs in the game repo:

**platforms.json:**

```json
"draugiem": {
    "appId": "[APP_ID]",
    "groupId": "[GAME_SHORT_NAME]page"
}
```

**secret_keys.json:**

```json
"draugiem": {
    "appKey": "[APP_KEY]"
}
```

3.2 Commit and push the changes.

3.3 Ask the developer to apply configs and redeploy:

```
@akargapolov Привет! Добавил конфиг для [GAME_SHORT_NAME] в Draugiem (platforms.json + secret_keys.json). Запусти, пожалуйста:

bash cleverapps/common/cacheconfigs/cacheconfigs.bash

И передеплой сервер.

Как будет готово, пришли скриншот в чат.
```

🖼️ Result example: https://gyazo.com/5a88da4a572b674d7fd5732ae21c9602

---

## 4. App information & settings (Manager)

4.1 Go to frype.com → your app → **Edit** and fill in:

- Application URL: `https://[APP_URL]`
- Google Play address: `[GOOGLE_PLAY_URL]`
- App Store address: `[APP_STORE_URL]` (⚠️ must start with `itunes.apple.com/lv`)
- Default iframe height: `620`
- Support e-mail: `help.cleverapps@gmail.com`
- Developer title: `Clever Apps Pte. Ltd.`

4.2 Fill in the mobile URL field:

- **Application URL (mobile web):** `https://[APP_URL]` — same value as Application URL. This field controls game availability in the mobile version of draugiem.lv.

> ℹ️ Чекбокс "Works on mobile devices" (Aplikācija darbojas mobilajās iekārtās) отсутствует на frype.com — мобильная поддержка управляется через поле "Application URL (mobile web)" выше.

4.3 Check the boxes:

- ☑ **Show app in full width** (Rādīt aplikāciju pilnā platumā)

4.4 Permissions — check:

- ☑ Name (Vārds)
- ☑ Surname (Uzvārds)
- ☑ Picture (Bilde)

> ℹ️ Поле "Kāpēc šie dati ir nepieciešami?" may appear only after enabling Permissions. If it appears, enter: `We use persons name and picture to list in players top in tournaments and to view friends progress`

4.5 Don't forget to save changes at the bottom of the screen.

🖼️ Result example: *(add screenshot)*

---

## 5. Promo materials (Manager)

5.1 Go to the Continuous Integration chat of the game (CI Merge for merge-3 games, AiSensia CI for yatzy games, etc). Request access if necessary:

```
@akargapolov добавь меня пожалуйста в чат CI Merge
```

5.2 Send command to the bot:

```
[CI_BOT] generateicons [GAME_SHORT_NAME] draugiem
```

Choose the filters: "All", add event: "No". Bot will report that assets have been generated.

🖼️ Result example: https://gyazo.com/a8349a1d067b271416b73b446a6a0cdd

Generated assets will appear in `promo/draugiem`.

🖼️ Result example: https://gyazo.com/780629ce48791373093b83753a673ad8

5.3 Upload assets to Draugiem dashboard (frype.com → app → Edit):

| Field | Size |
|---|---|
| Image | 240×180 |
| Small image | 240×180 |
| Icon | 16×16 |
| Banner | 370×230 |
| Spotlight image | 750×318 |
| Opa.lv image | 670×300 |
| Opa.lv spotlight image | 1400×900 |

> ⚠️ Поле **Small image (240×180)** присутствует на странице, но не упомянуто в оригинальной инструкции — не забудьте его заполнить.

5.4 Click **Edit welcome page** and upload 5 game screenshots (format `fb_1920×1080`, language `lv`).

🖼️ Result example: *(add screenshot)*

---

## 6. Team access (Manager)

6.1 Click **Application members** and add by user profile link (frype.com → app → Application members):

Administrators:

| Пользователь | Профиль |
|---|---|
| Vsevolod Laskavyi | https://www.frype.com/user/5536860/ |
| Viacheslav Romanov | https://www.frype.com/user/5531190/ |

> ℹ️ При добавлении есть dropdown **Loma (роль)** — выберите **Administrator**.

---

## 7. Add IAP products (Manager)

> ℹ️ Paid Services activation by Olafs is handled in Prerequisites → iap enabled. By this point Paid Services should already be enabled on the account.

📄 Products are configured per game in its own platforms.json under `draugiem.iap` (use another game's draugiem.iap block as a template — e.g. [wondermerge/platforms.json](https://github.com/Clever-Apps-Pte-Ltd/wondermerge/blob/master/platforms.json)).

7.1 Go to frype.com → app → **Paid services** → add products manually.

For each product fill in:

- **Status callback URL:** `https://pay.cleverappssg.com/[GAME_SHORT_NAME]/draugiempayment`

> ⚠️ Используйте именно этот URL (HTTPS, через `pay.cleverappssg.com`). Старый формат `http://[GAME_SHORT_NAME].cleverappssg.com/heroes-rest/draugiempayment` — неверный.

After creating products, copy their IDs into `draugiem.iap` in platforms.json.

🖼️ Result example: *(add screenshot)*

7.2 After updating platforms.json, ask the developer to run cacheconfigs and redeploy again (same as step 3.3):

```
@akargapolov Привет! Обновил iap для [GAME_SHORT_NAME]. Запусти, пожалуйста, cacheconfigs и передеплой сервер. Как будет готово, пришли скриншот.
```

7.3 After deploy, open https://www.frype.com/[GAME_SHORT_NAME] and verify that a test purchase completes successfully.

🖼️ Result example: *(add screenshot)*

**Troubleshooting:**

❌ `Load products: 0` / покупка не проходит, валюта не зачисляется — на сервере задеплоена старая версия кода. Попросите `@akargapolov` задеплоить актуальную версию. После деплоя проверьте версию JS-файлов в консоли браузера, затем повторите тест.

❌ Сервер возвращает `{"code":"error"}` при валидации покупки — в продуктах на Draugiem прописан неверный Status callback URL. В каждом продукте (Paid services → Edit) исправьте URL на `https://pay.cleverappssg.com/[GAME_SHORT_NAME]/draugiempayment`.

---

## 8. Import reviewed translations & rebuild game (Manager + Developer)

> ℹ️ Exporting strings and sending them to Olafs for review is handled in Prerequisites → share localizations. This step is only the import after Olafs returns the reviewed sheet.

8.0 Перед импортом убедитесь что вы не затрёте более свежую версию — проверьте историю файла на GitHub:

```
https://github.com/Clever-Apps-Pte-Ltd/[GAME_SHORT_NAME]/commits/master/res/localization/lv.json
```

Если последний коммит уже содержит актуальные переводы — импорт не нужен.

8.1 Import the reviewed translations:

1. Откройте CI-чат игры (для merge-игр — CI Merge)
2. Напишите боту: `[CI_BOT] localize [GAME_SHORT_NAME]`
3. Выберите команду **import**
4. Вставьте URL таблицы в формате:
   ```
   https://docs.google.com/spreadsheets/d/[SPREADSHEET_ID]/edit#gid=[GID]
   ```
   > ⚠️ URL **обязательно должен содержать `#gid=ID`** (например `#gid=0`), иначе бот вернёт ошибку `Spreadsheet url must contain list ID`.
5. Дождитесь сообщения `finish`

После импорта откройте игру на frype.com, зайдите в Settings → Language → Latviešu и проверьте что UI отображается на латышском.

🖼️ Result example: *(add screenshot)*

8.2 Ask the developer to rebuild and redeploy the game so the new translations go live.

Переводы — мелкое изменение, поэтому делается **патч** от предыдущей production-ветки. Напишите разработчику:

```
@EvgenySenckewitch Залил переводы для [GAME_SHORT_NAME] (Latvian) в master. Создай, пожалуйста, патч и добавь туда эти изменения. Как будет готово, напиши в чат.
```

После подтверждения от разработчика — **не запускайте build самостоятельно**. Сборки делаются по расписанию и координируются с командой. Напишите в CI-чат:

```
Евгений сделал патч по переводам для [GAME_SHORT_NAME]. Можно ли включить в следующий деплой?
```

После деплоя проверьте латышский язык в игре на frype.com.

🖼️ Result example: *(add screenshot)*

---

## 9. Submit for approval (Manager)

9.1 Go to frype.com → app → Applications section, check **publish application** and click **Send for approval**.

> ⚠️ Перед отправкой убедитесь что загружена иконка **16×16** — без неё нельзя включить activities. Страница покажет: *"You can have activities enabled only when you add an 16×16 px icon."*

🖼️ Result example: *(add screenshot)*

9.2 Send email to request payments approval:

To: api@draugiem.lv  
CC: rvvslv@gmail.com, vr.laskavyi@gmail.com  
Subject: New application payments approval — [GAME_TITLE]

```
Hi!

Please approve payments for our new game: [GAME_TITLE],
application id: [APP_ID]

Have a good day!
```

---

## 10. Test & Launch (Manager)

**Olafs Kublinskis** — менеджер платформы Draugiem.lv. Он проверяет переводы, одобряет публикацию и инициирует промо со своей стороны.

10.1 Send Olafs:

- Описание игры на английском и латышском (можно взять из Google Play и перевести)
- Визуалы из папки `promo/draugiem/` репозитория

10.2 Typical questions from Olafs:

| Question | Answer |
|---|---|
| Will the game be available in draugiem.lv mobile version? | Yes — "Application URL (mobile web)" is set |
| Is the game available in fullscreen? | The game must implement it itself. Olafs recommends adding it (personal recommendation based on 11 years of user experience, not a platform requirement) |
| When will translations be updated? | Confirm patch status with the developer |

10.3 After moderation approval, verify:

- Игра загружается по публичной ссылке `https://www.frype.com/[GAME_SHORT_NAME]`
- UI отображается на латышском (Settings → Language → Latviešu)
- Магазин открывается, продукты видны
- Тестовая покупка проходит успешно
- Игра видна в каталоге приложений Draugiem

10.4 Notify Olafs that the game and translations are ready — he will start promo on his side.

🖼️ Result example: *(add screenshot)*
