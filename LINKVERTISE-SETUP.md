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

## Step 2 — Create ONE link

1. Dashboard → **Links** → **Create link** (some accounts show
   "New Link" / "Create publisher link").
2. **Destination URL** = your PlatoBoost key page. Use the same page your
   LootLabs wrap lands on, i.e. the URL from
   `https://api.platoboost.net/public/start` for service **31611**
   (the `auth.platorelay.com/...` style page). If you are unsure, open your
   LootLabs link once, complete its tasks and copy the page it finally opens —
   that is your destination.
3. Set the task/step count the same way you did on LootLabs (2–3 steps).
4. Save → Linkvertise gives you a short link such as
   `https://link-hub.net/123456/your-key-page` (or `link-target.net`,
   `direct-link.net`, `link-to.net` … any of them is fine).

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
