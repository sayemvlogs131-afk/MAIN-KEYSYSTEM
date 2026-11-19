# LINKVERTISE SETUP — 24 / 48 / 72 HOUR CHECKPOINT PLANS

FLOWER HUB v2.27.0 gives **both Linkvertise and LootLabs** three key
durations: **24h direct**, **48h with 3 checkpoints**, and **72h with 6
checkpoints**.

| Plan | Completion count | Checkpoints | Final page |
| --- | --- | --- | --- |
| 24 hours | 1 Linkvertise task or 1 LootLabs run | direct | `keypage.html?plan=24` |
| 48 hours | 3 completions | **3** | `keypage.html?plan=48&final=1` |
| 72 hours | 6 completions | **6** | `keypage.html?plan=72&final=1` |

The 48h and 72h Linkvertise links are **one-task links repeated by the
checkpoint page**. The page counts each return; after the final checkpoint it
redirects to `keypage.html?plan=48&final=1` or `?plan=72&final=1`.

LootLabs has its own checkpoint loop. The fan opens `lootstart.html?plan=48`
or `?plan=72`; that page remembers the plan, opens the LootLabs task link, and
`lootreturn.html` sends the fan to the LootLabs checkpoint page. LootLabs
continues with the same start page until `3/3` or `6/6`.

> LootLabs currently has **3 tasks per run**. One LootLabs run counts as one
> checkpoint, so 48h = 3 LootLabs runs (9 tasks) and 72h = 6 runs (18 tasks).
> If you want one individual task per checkpoint, set the LootLabs link to
> **1 task** in its dashboard, then regenerate the wrapped URL in
> `lootstart.html`.

> **Important:** a 3-task or 6-task Linkvertise link does not become three or
> six checkpoints. It completes all of its tasks in one visit and the page
> counts it as **one** checkpoint. Set each Linkvertise link to **1 task**.

---

## 0. Keep your existing short links

Do **not** create new short links. Use these same URLs and edit the settings
inside each existing Linkvertise link:

```text
24h: https://link-center.net/8313449/3r0W6fwK8KTy
48h: https://link-target.net/8313449/g4aUAmoXzmYf
72h: https://direct-link.net/8313449/TERfo1qdTk65
```

For each link, open **Edit link → Tasks** and set the task count to **1**.
Then set its destination:

```text
24h -> https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html?plan=24
48h -> https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/checkpoint.html?plan=48&done=1
72h -> https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/checkpoint.html?plan=72&done=1
```

Keep the short links themselves unchanged. The checkpoint page repeats the
same 48h/72h URL once per checkpoint; it does not need different URLs.

---

## 1. If you are creating links from scratch

In **https://creators.linkvertise.com/**:

1. **24h** — create a link with **1 task** and destination:
   ```text
   https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/keypage.html?plan=24
   ```
2. **48h** — create a link with **1 task** and destination:
   ```text
   https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/checkpoint.html?plan=48&done=1
   ```
3. **72h** — create a link with **1 task** and destination:
   ```text
   https://sayemvlogs131-afk.github.io/MAIN-KEYSYSTEM/checkpoint.html?plan=72&done=1
   ```

The `done=1` marker is what tells the checkpoint page that one Linkvertise task
was completed. Without it, the progress page will not advance.

## 2. Paste the short links into CONFIG

In `RBX_KEY_SYSTEM.lua`:

```lua
LinkvertisePlans = {
    [24] = "https://your-1-task-24h-link",
    [48] = "https://your-1-task-48h-link",
    [72] = "https://your-1-task-72h-link",
},
```

`LinkvertisePlans[24]` may stay empty; it falls back to `LinkvertiseLink`.
An empty slot shows `SETUP NEEDED`; the other plans still work.

The checkpoint page contains the 48h/72h short links in its provider map. If
you replace a short link in `LinkvertisePlans`, update the matching entry in
`checkpoint.html` too. LootLabs uses `lootstart.html?plan=48` / `?plan=72` in
that same map.

## 3. Publish

From the private repo root:

```bash
python tools/verify_hubs.py
python tools/obfuscate_hubs.py
python tools/verify_protected.py
python tools/run_smoke_protected.py
```

Copy `build/*` and the changed `web/*` files to the public repo, commit and
push. A private-repo push alone does not update fans.

## 4. What the fan sees

### 24 hours

1. Press **GET KEY**.
2. Pick **LINKVERTISE**.
3. Pick **24 HOURS / 1 TASK**.
4. Complete the Linkvertise task.
5. Land on `keypage.html?plan=24`.
6. Pass the human check, copy `FLOWER-XXXXX-XXXX`.

### 48 hours — 3 checkpoints

1. Pick **48 HOURS / 3 CHECKPOINTS**. The 48h short link opens.
2. Complete one Linkvertise task. Linkvertise returns to:
   `checkpoint.html?plan=48&done=1`
3. The page shows:
   `Progress: 1/3 Completed — You need to complete the Linkvertise task 2 more times.`
4. Press **CONTINUE TO TASK 2**. It opens the same 48h short link.
5. After task 2: `2/3 Completed — 1 more time`.
6. After task 3: the page redirects to
   `keypage.html?plan=48&final=1`.
7. Pass the human check, copy `FLOWER48-XXXXX-XXXX`.

### 72 hours — 6 checkpoints

Same loop with six completions:

```text
1/6  -> 2/6 -> 3/6 -> 4/6 -> 5/6 -> 6/6 -> final key page
```

After the sixth return the page redirects to
`keypage.html?plan=72&final=1`; the key is `FLOWER72-XXXXX-XXXX`.

### LootLabs — 24h / 48h / 72h

1. Press **GET KEY** → **LOOTLABS** → choose **24h**, **48h**, or **72h**.
2. The GUI copies `lootstart.html?plan=NH`.
3. `lootstart.html` remembers the plan and opens your LootLabs link.
4. After the LootLabs run, `lootreturn.html` sends the fan to:
   - `keypage.html?plan=24` for 24h, or
   - `checkpoint.html?plan=NH&provider=lootlabs&done=1` for 48h/72h.
5. The checkpoint page continues with `lootstart.html?plan=48` or `?plan=72`
   until the last checkpoint, then opens the timed final key page.

**LootLabs setup:** no new short link is needed. The wrapped LootLabs URL is
inside `lootstart.html` and its encrypted destination is
`lootreturn.html`. If you replace your LootLabs short link, regenerate that
wrapped URL and keep `CONFIG.LootLabsStaticLink` in the script in sync.

## 5. Testing

- `GET KEY` → `LINKVERTISE` → all three plan rows visible.
- 24h: badge shows `24 HOUR KEY - 1 TASK`; open the 24h link and confirm it
  lands on `keypage.html?plan=24`.
- 48h: open `checkpoint.html?plan=48`; it must show `0 / 3` and **START TASK 1**.
- Add `&done=1` once; it must show `1 / 3`, **CONTINUE TO TASK 2**, and remove
  `done=1` from the address. Refresh must stay at `1 / 3`.
- Add `&done=1` twice more; the page must reach `3 / 3` and redirect to
  `keypage.html?plan=48&final=1`.
- 72h: `checkpoint.html?plan=72` must show `0 / 6`; after six `done=1` returns
  it must redirect to `keypage.html?plan=72&final=1`.
- Direct `keypage.html?plan=48` (without `final=1`) must redirect to the 48h
  checkpoint page.
- The final page must show only the human check, then the key — no extra task
  buttons.
- Check that the 48h/72h Linkvertise links are **1 task** in the Linkvertise
  dashboard. A 3-task link still only produces one `done=1` return.
- LootLabs: `GET KEY` → `LOOTLABS` → all three plan rows visible. Choosing 48h
  copies `lootstart.html?plan=48`; complete a run and confirm it lands on
  `checkpoint.html?plan=48&provider=lootlabs`, showing `1 / 3` and
  **CONTINUE TO RUN 2**. Repeat to the final 48h page.
- LootLabs 72h must show `0 / 6`, then `1 / 6`, and offer **CONTINUE TO RUN 2**.
- Linkvertise 48h/72h must still show `TASK` wording and keep its old progress
  key; LootLabs progress is stored separately under `_lootlabs`.

## 6. Important limitation

The checkpoint count is stored in the browser (`localStorage`) and the
`done=1` marker is public. A user who opens `checkpoint.html?plan=72` directly
can add `done=1` six times without doing the Linkvertise tasks, and a user who
opens `keypage.html?plan=72&final=1` directly can skip the gate. Stopping that
requires a server-side Linkvertise verifier; the current GitHub-only setup has
no backend.

Linkvertise's own task screen still shows how many steps are left inside a
single link. Our checkpoint page shows how many **Linkvertise completions**
remain for the 48h/72h plan.

## 7. Troubleshooting

| Problem | Fix |
| --- | --- |
| 48H/72H says `SETUP NEEDED` | Fill the matching slot in `LinkvertisePlans` and rebuild. |
| Progress stays at 0 after a task | The Linkvertise destination is missing `?plan=48&done=1` (or `72`). Edit the destination. |
| Progress jumps too far | The link has multiple tasks; set the Linkvertise link to **1 task**. |
| Refresh adds progress | The page did not strip `done=1`; hard-refresh and re-check the destination. |
| Fan gets a 24h key from the 48h link | The destination is missing `?plan=48`; edit it to the 48h checkpoint URL. |
| Key page redirects to checkpoints | Expected for 48h/72h until `final=1` is present. |
| Key rejected | Copy the whole `FLOWER48-...` / `FLOWER72-...` key; the last 4 characters are a checksum. |
| Link pays 0 | Payout details are missing on the Linkvertise dashboard. |
