# StreamMod — Moderator Dashboard

A free Windows desktop app for moderating Twitch (and YouTube) live chats, monitoring a Discord mod channel, and controlling music playback via VLC — all from one panel.

This repo hosts the **downloadable installer only**. This guide covers everything you need for first-time setup — for anything not covered here, ask whoever gave you the download link.

---

## Download & Install

1. Go to the [Releases page](https://github.com/dragonsniper43/Mod-Dashboard-Releases/releases/latest) (or click **Latest release** in the sidebar of this page).
2. Under **Assets**, download `StreamMod-Setup-X.Y.Z.exe`.
3. Run the downloaded file. Windows will likely show a blue **"Windows protected your PC"** screen — this is normal for a new app that isn't code-signed yet, not a sign of anything wrong. Click **More info**, then **Run anyway**.
4. Follow the installer wizard (you can choose whether to add a desktop shortcut). When it finishes, StreamMod launches automatically.

No Python, no command line, nothing else to install — the installer includes everything StreamMod needs.

---

## First-Time Setup

The first time StreamMod opens, a setup wizard walks you through connecting your Twitch account. Everything else (YouTube, Discord, VLC) is optional and can be set up later from **⚙ Settings**.

### Twitch (required)

StreamMod needs a free Twitch Developer Application so it can read and moderate your chat.

1. Go to [dev.twitch.tv/console](https://dev.twitch.tv/console) and log in with your Twitch account.
2. Click **Register Your Application**.
3. Fill in:
   - **Name:** anything, e.g. `StreamMod`
   - **OAuth Redirect URLs:** `http://localhost:3000/callback`
   - **Category:** Chat Bot (or Other)
4. Click **Create**, then open the application you just created.
5. Copy the **Client ID** shown on the page.
6. Back in StreamMod's setup wizard, paste the **Client ID** into the Twitch field and click **Connect Twitch**.
7. A browser window opens asking you to authorise the app — click **Authorize**. You're connected.

Once connected, click the **⊞ Channels** button (top toolbar) to add the Twitch channel(s) you want to monitor.

### YouTube (optional)

Lets you read YouTube Live Chat alongside Twitch chat, in the same window.

1. Go to [console.cloud.google.com](https://console.cloud.google.com) and create a new project (or use an existing one).
2. Go to **APIs & Services → Library**, search for **YouTube Data API v3**, and click **Enable**.
3. Go to **APIs & Services → Credentials → Create Credentials → API key**, then copy the key.
4. In StreamMod, click **⚙ Settings → YouTube**, paste the API key, and enter the channel URL or @handle of the YouTube stream you want to watch.

To also *send* messages into YouTube chat, not just read them, you'll need one extra OAuth step:

1. Go to Google Cloud Console → create a new project (or use an existing one).,
2. Enable the YouTube Data API v3 for that project.,
3. Go to APIs & Services → OAuth consent screen → set it up (External, fill required fields) → leave it in Testing mode.,
4. Under Test users, add your own Google account email (must add yourself since it's your project).,
5. Go to APIs & Services → Credentials → Create Credentials → OAuth client ID → type Desktop app (check what type _google_device_flow expects — it's a device-code flow, so Desktop/TV type is right).,
6. Copy the generated Client ID and Client Secret.,
7. In the dashboard: Settings → YouTube → paste Client ID/Secret → click connect/auth.

### Discord (optional)

Lets you monitor a private Discord channel (e.g. a mod room) inside StreamMod, send messages to it, and auto-post Twitch mod actions (bans/timeouts/warns) there.

> **Check with your team first.** Only one Discord bot is needed per team. If your team already set one up for StreamMod, just ask your admin for the existing **Bot Token** and skip to Step 4 below — do **not** create a second bot.

**Step 1 — Create a bot (skip if your team already has one):**
1. Go to [discord.com/developers/applications](https://discord.com/developers/applications) and click **New Application**.
2. Name it (e.g. `StreamMod`) and click **Create**.
3. In the left sidebar, click **Bot**, then **Add Bot**.
4. Under **Privileged Gateway Intents**, enable **Message Content Intent**, then **Save Changes**.
5. Click **Copy** under the token field — this is your **Bot Token**. Keep it private, like a password.

**Step 2 — Invite the bot to your server:**
1. In the left sidebar, click **OAuth2 → URL Generator**.
2. Under **Scopes**, tick `bot`. Under **Bot Permissions**, tick `Read Messages/View Channels` and `Send Messages`.
3. Copy the generated URL, open it in a browser, and invite the bot to your server.

**Step 3 — Get your channel ID:**
1. In Discord, go to **User Settings → Advanced** and enable **Developer Mode**.
2. Right-click the channel you want to monitor and choose **Copy Channel ID**.

**Step 4 — Connect in StreamMod:**
1. Click **⚙ Settings → Discord**.
2. Paste the **Bot Token** and the **Channel ID** (one per line).
3. Click **▶ Connect Discord**.

### VLC (optional)

If you play music through VLC while streaming, StreamMod can show a player bar with playback controls and let chat interact with it (`!song`, and mod-only `!play`/`!pause`/`!next`/`!skip`).

1. In VLC, go to **Tools → Preferences**, set **Show settings** (bottom-left) to **All**.
2. Go to **Interface → Main interfaces**, tick **Web**.
3. Expand **Main interfaces → Lua**, set an **HTTP password**, click **Save**, then restart VLC.
4. In StreamMod, go to **⚙ Settings → VLC**, enter the password (default port `8080`), and click **Test connection**.

---

## Updates

StreamMod checks for new versions automatically and can install them with one click. You can also check manually any time via **⚙ Settings → General → About → Check for updates**.

---

## Privacy

StreamMod stores your settings and account tokens locally on your own PC — there's no StreamMod-operated server your data passes through. See the full [Privacy Policy](https://dragonsniper43.github.io/privacy-policy.html) for exactly what each connected service (Twitch, Google/YouTube, Discord) is used for.

---

## Troubleshooting

**Windows blocked the installer / "Windows protected your PC"**
This is Microsoft SmartScreen flagging a new, not-yet-code-signed app — it doesn't mean anything is wrong. Click **More info → Run anyway**.

**App won't start after install**
Make sure the WebView2 Runtime is installed — nearly all Windows 10/11 PCs already have it since Microsoft Edge ships it by default. The installer checks for this automatically.

**Chat not connecting / stuck reconnecting**
Go to **⚙ Settings → Twitch → Reconnect Twitch** to refresh your login. If it keeps happening, your Twitch session may need a full re-authorise via the same button.

**Discord panel won't connect**
- Double-check the bot token was copied in full with no extra spaces.
- Make sure **Message Content Intent** is enabled for the bot (Discord Developer Portal → Bot → Privileged Gateway Intents).
- Confirm the bot was actually invited to your server.
- Make sure the channel ID is the numeric ID (not the channel name) — right-click the channel in Discord with Developer Mode on, then **Copy Channel ID**.

**Still stuck?**
Check the log file at `%LOCALAPPDATA%\StreamMod\mod_debug.log` for details, or reach out to whoever gave you the download link.
