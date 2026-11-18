# NightLore "open in app" link

A free replacement for LinkTwin: one static `index.html` (no build, no dependencies). Someone taps your link inside Instagram, TikTok or
Facebook, and the page opens the **YouTube app** instead of YouTube inside the in-app browser.

- **Android:** an `intent://` link aimed at the YouTube app (`com.google.android.youtube`). If the app isn't installed, it falls back
  to the normal web page.
- **iPhone / iPad:** tries `youtube://`. After a tap, if the app hasn't opened within ~1.5 s, it falls back to the web page.
- **Computer:** goes straight to the YouTube web page.
- **Always on screen:** a big **Open in YouTube** button, because some in-app browsers block an automatic jump that doesn't come from a
  tap. Below it, a small **Open in browser** link as a last resort.
- **On arrival:** if an in-app browser blocks the first automatic try, the page stays put with the button. It doesn't jump to the
  website, because that would hide the button.

Links:
- `https://YOUR-DOMAIN/` opens the channel.
- `https://YOUR-DOMAIN/?v=VIDEO_ID` opens one video, for example `?v=fEsYAluikdo`. Anything that isn't an 11-character video ID is
  ignored and the channel opens instead.

## Change the settings

Open `index.html` and find the `CONFIG` block at the top of the script. You can change:
- the channel ID;
- the page title and the subtitle;
- the button labels;
- the language;
- the colours;
- the iPhone fallback delay.

**Hebrew:** set the labels in Hebrew and `lang: 'he'`, for example:
```js
openLabel: 'פתח ביוטיוב', browserLabel: 'פתח בדפדפן', subtitle: 'פותח את יוטיוב…', lang: 'he',
```
The text turns right-to-left by itself.

**Link preview (title, description, picture):** these are the `og:` and `twitter:` lines in `<head>`. Instagram, Facebook and WhatsApp
read them without running JavaScript, so they are edited by hand, not in CONFIG.
1. Replace `YOUR-DOMAIN` with your address.
2. Add a 1200x630 picture named `preview.jpg` next to `index.html`.

**Click counter (later):** the `track()` function has a commented example line. Nothing is tracked now.

## Put it online with GitHub Pages (free)

1. Make a GitHub account if you don't have one: https://github.com/signup
2. Create an empty repository at https://github.com/new
   - name: `nightlore-link`
   - Public
   - don't add a README; this folder already has one
3. Push this folder (the exact commands are at the end of this file).
4. On GitHub, open the repository's **Settings** tab, then **Pages**:
   - **Source:** Deploy from a branch
   - **Branch:** `main`
   - **Folder:** `/ (root)`
   - Click **Save**.
5. Wait a minute or two. Your page is at `https://YOUR-GITHUB-NAME.github.io/nightlore-link/`. Open it on a computer: it should go
   straight to your channel.
6. Put that address in the `og:url` / `og:image` lines (see "Link preview" above), then commit and push again.

**Vercel instead:** at https://vercel.com/new, import the repository and click Deploy. It needs no settings.

## Use your own domain (optional)

For example `go.nightlore.com`.
1. Buy a domain anywhere: Namecheap, Cloudflare, Porkbun.
2. In the domain's DNS settings, add a **CNAME** record:
   - name: `go`
   - value: `YOUR-GITHUB-NAME.github.io`
3. On GitHub, open **Settings**, then **Pages**, then **Custom domain**. Type `go.nightlore.com` and Save. GitHub adds a `CNAME` file to
   the repository: pull it (`git pull`) before your next push.
4. When the check turns green (DNS can take up to a few hours), tick **Enforce HTTPS**.
5. **For a bare domain** like `nightlore.com` instead: add four **A** records pointing to:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`

## Test it

**Before you test:** make sure the YouTube app is installed and you're logged in. Use your real address (`https://...`), not a file on
your computer.

**Instagram, both phones.** Put the link in your **bio**, and also as a **link sticker** on a story (you can post it to Close Friends
only). Then tap it:
- **Android:** the YouTube app should open on the channel. If the page stays up, tap **Open in YouTube**.
- **iPhone:** iOS may ask *"Open in YouTube?"*: tap **Open**. If the page stays up, tap **Open in YouTube**. If the app still doesn't
  open within ~1.5 s, the YouTube website loads instead.

**TikTok and Facebook:** test the same way, from a profile link and from a message to yourself.

**One video:** test a `?v=VIDEO_ID` link too. The video should open, not the channel.

### If the app doesn't open

- **Nothing happens on arrival:** that's the in-app browser blocking automatic jumps, which TikTok does often. The button is there for
  this: a tap counts as permission.
- **Android opens the website instead of the app:**
  1. Check the YouTube app is enabled: Settings → Apps → YouTube.
  2. Under **Open by default**, check that **Open supported links** is on.
  3. Then try again.
- **iPhone opens the website instead of the app:**
  - The in-app browser blocked `youtube://`. Tap the button again; some apps only allow it on a tap.
  - If it never works in one app, use that app's menu (•••) and choose **Open in browser** (Safari), then tap the button there.
  - Also check that the YouTube app isn't restricted (Settings → Screen Time → Content & Privacy → Allowed Apps).
- **The preview shows old text or no picture:**
  - Check that `preview.jpg` really loads at `https://YOUR-DOMAIN/preview.jpg`.
  - Refresh the preview in Facebook's Sharing Debugger (https://developers.facebook.com/tools/debug/): paste your link and click
    **Scrape Again**. Instagram uses the same cache.
- **The page changed but the phone shows the old one:** GitHub Pages caches for ~10 minutes. Add `?x=1` to the link to test the new one.
- **Honest limit:** Android's YouTube-app link is reliable. On iPhone, whether an in-app browser lets a page open another app is up to
  Instagram, TikTok or Facebook, and it changes with their updates. The button-plus-fallback covers those cases.

## Push it: the exact commands

The repository is already created and committed in this folder. Replace `YOUR-GITHUB-NAME`:

```bash
cd "E:/Desktop/youtube channel/nightlore-link"
git remote add origin https://github.com/YOUR-GITHUB-NAME/nightlore-link.git
git push -u origin main
```

**Before the first push**, set your own name and GitHub email on the commit. The commit was made with a placeholder identity. Find your
private address at https://github.com/settings/emails; it looks like `12345678+name@users.noreply.github.com`.
```bash
git config user.name "YOUR NAME"
git config user.email "YOUR-ID+YOUR-GITHUB-NAME@users.noreply.github.com"
git commit --amend --reset-author --no-edit
```

Then turn on Pages: **Settings**, then **Pages**, then the `main` branch and the `/ (root)` folder, then **Save**. Or from a terminal
with the GitHub CLI:
```bash
gh api -X POST repos/YOUR-GITHUB-NAME/nightlore-link/pages -f "source[branch]=main" -f "source[path]=/"
```
