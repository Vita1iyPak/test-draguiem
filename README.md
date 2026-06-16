# Draugiem Platform Setup
⏰ Estimated Time: ~2–3 hours (excluding waiting time for Olafs approval and Latvian translation review)

Replace the placeholders below with your game's values before you start:

| Placeholder | What to substitute | Example (Castle Craft) |
|---|---|---|
| `[GAME_TITLE]` | Public game title | Castle Craft |
| `[GAME_SHORT_NAME]` | Working title (GitHub) | hustlemerge |
| `[APP_ID]` | App ID on frype.com | 15021196 |
| `[APP_URL]` | Production HTML5 build URL | `hustlemerge.cleverappssg.com/publish/html5/` |
| `[GOOGLE_PLAY_URL]` | Google Play link | `play.google.com/store/apps/details?id=com.cleverapps.hustlemerge` |
| `[APP_STORE_URL]` | App Store link (must start with `itunes.apple.com/lv/...`) | `itunes.apple.com/lv/app/id6670314553` |
| `[APP_KEY]` | API key from the app dashboard | 991742c1ec70574d95ca2119de38309a |
| `[CI_BOT]` | Telegram CI bot for your game | `@cleverapps_stands_bot` (for merge games) |

---

## 1. Prerequisites

Copy this checklist and rename it for a specific game. Once all of the steps from Draugiem Tab are completed, attach the spreadsheet to the task and proceed with the integration.

[Checklist](https://docs.google.com/spreadsheets/d/1_PlNiliAx0hxyWPyZv-UYQal89YJUJnb9OzYrWuwwGY/edit?gid=1527026538#gid=1527026538)

Make sure the following is done before you start:

- [ ] **Account access** — you have the manager account login/password for frype.com (stored in the checklist)
- [ ] **iap enabled** — Paid Services are enabled for the app (activated by Olafs)
- [ ] **share localizations** — the Latvian translations spreadsheet is prepared and sent to Olafs for review
- [ ] **list of promos** — `draugiemproducts.js` is added to the repo (`cleverapps/src/[GAME_SHORT_NAME]/draugiemproducts.js`) — ask the developer
- [ ] **Assets** — all Draugiem promo materials are ready (see step 5). ⚠️ `banner_1920x1080` must be `.png`, not `.jpg`. If you have `.jpg` — ask the Launch team artists (`@anton_prokudiin`) to provide `.png`.

---

## 2. Create app on Draugiem (Manager)

> ℹ️ The platform is available at two domains: **draugiem.lv** and **frype.com** — they are the same. Use **frype.com** (it has an English interface).

2.1 Go to https://www.frype.com/applications/dev/myapps/ and click **Create Integrated Application**.

2.2 Fill in the form:

- Title: `[GAME_TITLE]`
- Short name: `[GAME_SHORT_NAME]` (latin only, lowercase)
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

> ℹ️ The "Works on mobile devices" checkbox (Aplikācija darbojas mobilajās iekārtās) is absent on frype.com — mobile support is controlled via the "Application URL (mobile web)" field above.

4.3 Check the boxes:

- ☑ **Show app in full width** (Rādīt aplikāciju pilnā platumā)

4.4 Permissions — check:

- ☑ Name (Vārds)
- ☑ Surname (Uzvārds)
- ☑ Picture (Bilde)

> ℹ️ The "Kāpēc šie dati ir nepieciešami?" field may appear only after enabling Permissions. If it appears, enter: `We use persons name and picture to list in players top in tournaments and to view friends progress`

4.5 Don't forget to save changes at the bottom of the screen.

🖼️ Result example: https://gyazo.com/94790147ed5cdab3376120c4fdef11f0

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

5.4 Click **Edit welcome page** and upload 5 game screenshots (format `fb_1920×1080`, language `lv`).

🖼️ Result example: https://gyazo.com/eecb4e2475a4573a8cbcd315d98bfa45

---

## 6. Team access (Manager)

6.1 Click **Application members** and add by user profile link (frype.com → app → Application members):

Administrators:

| User | Profile |
|---|---|
| Vsevolod Laskavyi | https://www.frype.com/user/5536860/ |
| Viacheslav Romanov | https://www.frype.com/user/5531190/ |

> ℹ️ There is a **Loma (role)** dropdown when adding — select **Administrator**.

---

## 7. Add IAP products (Manager)

> ℹ️ Paid Services activation by Olafs is handled in Prerequisites → iap enabled. By this point Paid Services should already be enabled on the account.

📄 Products are configured per game in its own platforms.json under `draugiem.iap` (use another game's draugiem.iap block as a template — e.g. [wondermerge/platforms.json](https://github.com/Clever-Apps-Pte-Ltd/wondermerge/blob/master/platforms.json)).

7.1 Go to frype.com → app → **Paid services** → add products manually.

For each product fill in:

- **Status callback URL:** `https://pay.cleverappssg.com/[GAME_SHORT_NAME]/draugiempayment`

> ⚠️ Use exactly this URL (HTTPS, via `pay.cleverappssg.com`). The old format `http://[GAME_SHORT_NAME].cleverappssg.com/heroes-rest/draugiempayment` is incorrect.

After creating products, copy their IDs into `draugiem.iap` in platforms.json.

🖼️ Result example: https://gyazo.com/e7e431c899ca7e44a786a61d9b00b5b4

7.2 After updating platforms.json, ask the developer to run cacheconfigs and redeploy again (same as step 3.3):

```
@akargapolov Привет! Обновил iap для [GAME_SHORT_NAME]. Запусти, пожалуйста, cacheconfigs и передеплой сервер. Как будет готово, пришли скриншот.
```

7.3 After deploy, open https://www.frype.com/[GAME_SHORT_NAME] and verify that a test purchase completes successfully.

🖼️ Result example: https://gyazo.com/3d2e2683d2c7491a01a9e0688aa5ba17

**Troubleshooting:**

❌ `Load products: 0` / purchase fails, currency not credited — the server has an outdated version deployed. Ask `@akargapolov` to deploy the latest version. After deploy, check the JS file version in the browser console, then retest.

❌ Server returns `{"code":"error"}` when validating a purchase — the Status callback URL in Draugiem products is incorrect. In each product (Paid services → Edit) fix the URL to `https://pay.cleverappssg.com/[GAME_SHORT_NAME]/draugiempayment`.

---

## 8. Import reviewed translations & rebuild game (Manager + Developer)

> ℹ️ Exporting strings and sending them to Olafs for review is handled in Prerequisites → share localizations. This step is only the import after Olafs returns the reviewed sheet.

8.1 Import the reviewed translations:

1. Open the CI chat of the game (CI Merge for merge games)
2. Send to the bot: `[CI_BOT] localize [GAME_SHORT_NAME]`
3. Select command **import**
4. Paste the spreadsheet URL in the format:
   ```
   https://docs.google.com/spreadsheets/d/[SPREADSHEET_ID]/edit#gid=[GID]
   ```
   > ⚠️ The URL **must contain `#gid=ID`** (e.g. `#gid=0`), otherwise the bot will return error `Spreadsheet url must contain list ID`.
5. Wait for the `finish` message

🖼️ Result example: https://gyazo.com/626f5346f44b844237c0aaae5ef7c727

8.2 Ask the developer to rebuild and redeploy the game so the new translations go live.

Translations are a small change, so a **patch** is made from the previous production branch. Message the developer with the commit link from GitHub:

```
@EvgenySenckewitch Залил переводы для [GAME_SHORT_NAME] (Latvian) в master. Создай, пожалуйста, патч и добавь туда эти изменения. Как будет готово, напиши в чат. Вот коммит https://github.com/Clever-Apps-Pte-Ltd/hustlemerge/commit/e859a6d627e60d843d11f7510b98d710dd9c0e58
```

After the developer confirms — **do not trigger a build yourself**. Builds are done on schedule and coordinated with the team. Post in the CI chat with the patch commit link:

```
Евгений сделал патч по переводам для [GAME_SHORT_NAME]. Ссылка на коммит с патчем https://github.com/Clever-Apps-Pte-Ltd/hustlemerge/commit/8a84ba092f561e7a70ef616ad384a1551d46b658 Можно ли включить в следующий деплой?
```
> ℹ️ If the scheduled deploy is not coming soon and you need to release translations urgently, trigger the build manually via the CI bot:
>
> 1. `[CI_BOT] directbuild [GAME_SHORT_NAME]` — adds the build task directly to the builder
> 2. `[CI_BOT] deploy [GAME_SHORT_NAME]` — deploys the release version

After deploy, verify Latvian language in the game on frype.com.

🖼️ Result example: https://gyazo.com/760df6e0cc356c7d5ed20e1000bec3f8

---

## 9. Submit for approval (Manager)

9.1 Go to frype.com → app → Applications section, check **publish application** and click **Send for approval**.

> ⚠️ Before submitting, make sure the **16×16** icon is uploaded — without it you cannot enable activities. The page will show: *"You can have activities enabled only when you add an 16×16 px icon."*

🖼️ Result example: https://gyazo.com/94790147ed5cdab3376120c4fdef11f0

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

**Olafs Kublinskis** — Draugiem.lv platform manager. He reviews translations, approves the publication, and initiates promo on his side.

10.1 Send Olafs:

- Game description in English and Latvian (can be taken from Google Play and translated)
- Visuals from the `promo/draugiem/` folder in the repo

10.2 Typical questions from Olafs:

| Question | Answer |
|---|---|
| Will the game be available in draugiem.lv mobile version? | Yes — "Application URL (mobile web)" is set |
| Is the game available in fullscreen? | The game must implement it itself. Olafs recommends adding it (personal recommendation based on 11 years of user experience, not a platform requirement) |
| When will translations be updated? | The translations are already updated and live. The game is ready to go public whenever you're ready to start the promo! |

10.3 After moderation approval, verify:

- Game loads at the public URL `https://www.frype.com/[GAME_SHORT_NAME]`
- UI is displayed in Latvian (Settings → Language → Latvijas)
- Shop opens, products are visible
- Test purchase completes successfully
- Game is visible in the Draugiem app catalog

10.4 Notify Olafs that the game and translations are ready — he will start promo on his side.

🖼️ Result example: https://gyazo.com/9d139ece5c1a5bb8bf650421160e4804
