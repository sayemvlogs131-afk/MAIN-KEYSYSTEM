# LINKVERTISE SETUP — one time, ~3 minutes

Your key system now lets every fan pick **LOOTLABS** or **LINKVERTISE** when they
press **GET KEY**. LootLabs already works (your link + API key are in
`CONFIG`). Linkvertise needs one short link created on your own account — it
takes about 3 minutes and is free.

> I cannot create the account for you: Linkvertise requires YOUR email, login
> and payout details, and links only earn once your account is verified.
> Everything else is already wired — you only paste one link.

---

## Step 1 — Create the account

1. Open **https://creators.linkvertise.com/** (this is the publisher/creator
   login, not the ad-blocked visitor page).
2. Sign up with your Gmail and confirm the email.
3. In the dashboard fill in your **payout details** (country, payout method).
   Without payout details links still work but stay at 0 earnings.

## Step 2 — Set the DESTINATION (this is the important one)

Open your link `https://link-center.net/8313449/3r0W6fwK8KTy` in the Linkvertise
dashboard → **Edit link** → **Destination URL**, and paste exactly this:

```
https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html
```

Save it. That's the whole setup.

### Why this page and not PlatoBoost directly?

PlatoBoost key pages are **per-device** (`auth.platorelay.com/a?d=…`). A plain
PlatoBoost URL shows **"Expired link"** to anyone else — so a fixed Linkvertise
destination pointed there would give your fans no key at all.

`keypage.html` (already live, hosted from your own repo via GitHub Pages) asks
the fan for their **HUB CODE**, then mints **their own** key page on the spot and
opens it. Verified working: it lands on your live service page
("get key Flower hub — Completed 0 of 3").

## What the fan does

1. Key window → **GET KEY** → taps the **LINKVERTISE** card → link is copied.
2. They open it and complete the Linkvertise tasks → they keep earning you money.
3. They land on the FLOWER HUB key page, type/paste their **HUB CODE**
   (visible next to **COPY CODE** in the key window).
4. They get their personal PlatoBoost page, finish it, copy the key, paste it,
   press **VERIFY** — hub loads.

## Step 3 — Paste it into the script

In `RBX_KEY_SYSTEM.lua`, near the top:

```lua
    LinkvertiseLink    = "",   -- <-- paste the short link between the quotes
```

Example:

```lua
    LinkvertiseLink    = "https://link-hub.net/123456/your-key-page",
```

Save the file, re-obfuscate/re-upload it to GitHub (or run
`python obfuscate_hubs.py` in the project), and that's it.

## What the fans see

- **GET KEY** → a panel with two animated cards:
  - **LOOTLABS** — always shows `READY ✓`
  - **LINKVERTISE** — shows `READY ✓` once `LinkvertiseLink` is not empty,
    otherwise `SETUP NEEDED`
- Tapping a card copies that site's link to the clipboard and tells them the
  exact steps (tasks → key page → copy THE KEY → paste → VERIFY).

Both sites end on the **same PlatoBoost key page**, so a key earned on
Linkvertise verifies exactly like one earned on LootLabs. Nothing else in the
script has to change.

## Extra money tip

Create the Linkvertise link with the **same destination** as LootLabs so a fan
who prefers Linkvertise still passes through PlatoBoost afterwards — you keep
earning on both platforms no matter which card they tap.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Card says `SETUP NEEDED` | `LinkvertiseLink` is still empty — paste the short link. |
| Link opens but pays 0 | Payout details missing on the Linkvertise dashboard. |
| Link blocked / "unsafe" | Linkvertise reviews new links; wait a few minutes and try again. |
| Fans say the page is slow on phones | Tell them to tap LOOTLABS instead — both give the same key. |
