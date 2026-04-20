# rugs.art

new sacred artifacts volume 1: awareness
— 4 rugs deeply considered by kyle louis fletcher

a single-file static site. everything lives in `index.html` (markup + styles + script + artwork base64).

---

## going live on rugs.art — one-time setup (~10 minutes)

do these once, in order. after this, publishing edits is a single `git push`.

### 1. create a github repo

1. sign in to github.com (account: `kyleluvsew`).
2. click **new repository** (top right **+** menu).
3. name it: `rugs-art`
4. description: `new sacred artifacts vol 1 — kylefletcher.com`
5. **public** (required for free github pages).
6. **do not** check "add a readme" / "add .gitignore" / "add license" — we already have them.
7. click **create repository**.
8. leave that page open; copy the url it shows (it'll look like `https://github.com/kyleluvsew/rugs-art.git`).

### 2. push this folder

open terminal, paste these commands one by one from inside this folder:

```bash
cd "~/Development/Rugs Website"
git remote add origin https://github.com/kyleluvsew/rugs-art.git
git branch -M main
git push -u origin main
```

github will prompt for username + password. for the password, use a **personal access token**, not your real password:
- github → settings → developer settings → personal access tokens → tokens (classic) → **generate new token (classic)**
- scope: check **repo** only
- copy the token, paste it at the password prompt

(one-time friction. or set up ssh keys / gh cli if you prefer.)

### 3. turn on github pages

1. on your new repo page, click **settings** (top tab).
2. left sidebar → **pages**.
3. under **build and deployment** → **source**: select `deploy from a branch`.
4. branch: **main**, folder: **/ (root)**. click **save**.
5. scroll up. you'll see **custom domain**: type `rugs.art`, click **save**.
6. check **enforce https** once the option lights up (takes a few minutes while github provisions an ssl cert — grab coffee).

at this point github is serving the site, but rugs.art doesn't know to point at it yet. that's dns.

### 4. point namecheap dns at github

1. log in to namecheap.com → **domain list** → click **manage** next to `rugs.art`.
2. click the **advanced dns** tab.
3. delete any existing `a record` or `cname record` rows for `@` or `www` (github's records need to be the only ones). leave mx / txt records alone if you're using email on this domain.
4. click **add new record** four times to create these four **a records** (apex `@`, all pointing at github's pages servers):

| type     | host | value             | ttl      |
| -------- | ---- | ----------------- | -------- |
| a record | @    | 185.199.108.153   | automatic |
| a record | @    | 185.199.109.153   | automatic |
| a record | @    | 185.199.110.153   | automatic |
| a record | @    | 185.199.111.153   | automatic |

5. (optional but nice) add one **cname record** so `www.rugs.art` also works:

| type         | host | value                   | ttl       |
| ------------ | ---- | ----------------------- | --------- |
| cname record | www  | kyleluvsew.github.io.   | automatic |

6. click the green ✓ to save each row.

### 5. wait

dns propagation: usually 5–30 minutes, occasionally a few hours.
ssl provisioning (github side): up to an hour after dns resolves.

test with: `dig rugs.art +short` — it should return the four github ip addresses.

once https://rugs.art loads with a green padlock, you're live. 🎉

---

## making edits later

the whole site is `index.html`. edit it in any text editor. to publish:

```bash
git add index.html
git commit -m "description of what changed"
git push
```

github pages updates within 1–2 minutes of every push.

### common edits

- **change the btc wallet address:** search for `BTC_ADDRESS` in `index.html` — one line.
- **per-piece btc addresses (for better privacy):** search for `ADDR_OVERRIDES`, fill in a fresh address per piece.
- **mark a piece as sold:** find the piece's `<article class="plate"` and change `data-status="available"` to `data-status="sold"`. the green dot turns red and the acquire button is disabled.
- **change the usd price:** search for `USD_PRICE` (currently `4000`).
- **change contact email:** search for `CONTACT_EMAIL` and `hello@kylefletcher.com` (two spots).

---

## files

- `index.html` — the site (1 mb, includes all four rug images base64-embedded)
- `CNAME` — tells github pages the custom domain is `rugs.art`. do not delete.
- `.gitignore` — excludes macos / editor junk from the repo
- `README.md` — this file
