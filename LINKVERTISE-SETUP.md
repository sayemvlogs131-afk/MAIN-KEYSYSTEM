# LINKVERTISE SETUP — 24 / 48 / 72 HOUR PLANS

FLOWER HUB v2.23.0 gives Linkvertise three key durations. LootLabs stays
**24 hours only**.

| Linkvertise plan | Tasks | Destination |
| --- | --- | --- |
| 24 hours | 1 task | `https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html?plan=24` |
| 48 hours | 3 tasks | `https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html?plan=48` |
| 72 hours | 6 tasks | `https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html?plan=72` |

The task count is a property of the **Linkvertise link**. The hub cannot turn a
1-task link into a 6-task link, so you need three separate links.

Linkvertise shows the remaining steps on its own page between tasks. Our key
page only receives the final redirect, so it cannot display “2 tasks left”.

---

## 1. Create the three links

In **https://creators.linkvertise.com/**:

1. Create a link with **1 task** and the `?plan=24` destination above.
2. Create a second link with **3 tasks** and the `?plan=48` destination.
3. Create a third link with **6 tasks** and the `?plan=72` destination.

Use the exact destination for each link. If the `?plan=` part is missing, the
key page falls back to 24 hours.

## 2. Paste them into CONFIG

In `RBX_KEY_SYSTEM.lua`:

```lua
LinkvertisePlans = {
    [24] = "https://your-1-task-link",
    [48] = "https://your-3-task-link",
    [72] = "https://your-6-task-link",
},
```

Notes:

- `LinkvertisePlans[24]` may stay empty; it falls back to the existing
  `LinkvertiseLink`. That fallback link must still be the 1-task link for the
  correct task count.
- `LinkvertisePlans[48]` and `[72]` show `SETUP NEEDED` until filled. A fan can
  still use 24H or LootLabs; nothing breaks.
- Do not put a direct `keypage.html` URL in these slots unless you want the fan
  to skip Linkvertise. These slots hold the **short links**.

## 3. Rebuild and publish

From the private repo root:

```bash
python tools/verify_hubs.py
python tools/obfuscate_hubs.py
python tools/verify_protected.py
python tools/run_smoke_protected.py
```

Then copy `build/*` and the changed `web/*` files to the public repo, commit and
push. A private-repo push alone does not update fans.

## 4. What the fan sees

1. Press **GET KEY**.
2. Pick **LINKVERTISE**.
3. Pick **24 HOURS / 1 TASK**, **48 HOURS / 3 TASKS** or
   **72 HOURS / 6 TASKS**.
4. The matching short link is copied. They open it and complete the tasks;
   Linkvertise shows the remaining steps.
5. After the final task they land on `keypage.html?plan=...`.
6. The page shows the selected duration and task count, then mints:
   - 24h: `FLOWER-XXXXX-XXXX`
   - 48h: `FLOWER48-XXXXX-XXXX`
   - 72h: `FLOWER72-XXXXX-XXXX`
7. They paste the key into FLOWER HUB and press **VERIFY**. The key badge counts
   down the selected duration.

**LootLabs** is unchanged and always ends on the PlatoBoost key page for a
24-hour key.

## 5. Testing

- `GET KEY` -> `LINKVERTISE` -> all three plan rows must be visible.
- 24h should say `READY` if `LinkvertiseLink` is filled.
- 48h/72h should say `SETUP NEEDED` while their slots are empty.
- Open `keypage.html?plan=48`: the badge must say `48 HOUR KEY - 3 TASKS` and
  the format must say `FLOWER48-XXXXX-XXXX`.
- Open `keypage.html?plan=72`: it must say `72 HOUR KEY - 6 TASKS` and
  `FLOWER72-XXXXX-XXXX`.
- `keypage.html` without `?plan=` must stay on 24 hours.
- The protected smoke now checks that the plan chooser opens and all three plan
  buttons exist.

## 6. Important limitation

The 24/48/72 key math is shared by the key page and the Lua, so the keys verify
offline. But the website cannot prove which Linkvertise tasks were completed.
A user who knows the direct `keypage.html?plan=72` URL can mint a 72-hour key
without doing the 6 tasks. Stopping that requires a server-side Linkvertise
verifier; the current GitHub-only setup does not have one.

## 7. Troubleshooting

| Problem | Fix |
| --- | --- |
| 48H/72H says `SETUP NEEDED` | Paste the matching short link into `LinkvertisePlans` and rebuild. |
| Fan gets a 24h key from the 48h link | The Linkvertise destination is missing `?plan=48`; edit the destination. |
| Link opens the wrong task count | Linkvertise task count is per link; create/edit the correct link. |
| Key rejected | Copy the whole `FLOWER48-...` / `FLOWER72-...` key; the last 4 characters are a checksum. |
| Key expires early | Device clock/UTC mismatch, or the save was created before the plan change; get a fresh key. |
| Link pays 0 | Payout details are missing on the Linkvertise dashboard. |
