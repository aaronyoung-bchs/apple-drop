# Apple Drop

A harvest-festival drop game for the **Mu Alpha Theta November 2026 challenge** at Bullitt Central High School.

Teams drop apples, collect data, and decide: *is a drop worth the ticket?*

The whole game is one self-contained file, `index.html`. It has no build step, no server, no libraries, and no tracking.

## Publish on GitHub Pages

1. Merge this branch into `main`.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then pick `main` and `/ (root)`, and click **Save**.
4. After a minute or two, the game is live at `https://<your-username>.github.io/apple-drop/`.

To preview locally, just open `index.html` in a browser. Copying results needs the live `https://` page or a very recent browser, but the page has a fallback copy box if copying is blocked.

## How the game works

- At every peg, the apple goes left or right with equal chance (one fair coin flip, from `crypto.getRandomValues`).
- **Walls:** an apple on a peg next to a wall can't go toward it, so it goes the other way. No flip happens there.
- **Missing pegs** (drawn as dashed rings): no bounce. The apple falls straight down through the gap between the two pegs below.
- The animation is only for show. The outcome is decided first by `dropOneApple()`, and then replayed on screen.

## Building a new board

Edit only the `BOARD_CONFIG` object at the top of the `<script>` in `index.html`. Every setting has a comment.

| Setting | What it does |
| --- | --- |
| `rows` | Number of peg rows. |
| `chutes` | Where apples can enter. `offset` 0 = center, -1 = one peg left, +1 = one peg right. |
| `startMode` | `"fixed"` (always use `fixedChute`) or `"choice"` (player picks before each drop). |
| `walls.left` / `walls.right` | Row number where that wall begins, or `null` for no wall. |
| `missingPegs` | List of `{ row, peg }`. Rows count from the top and pegs from the left, both starting at 1. Not allowed in the last row. |
| `slots` | Prizes, left to right: `letter`, `name`, `value`. |
| `ticketPrice`, `unit` | Cost per drop and the unit's name. |
| `batchSize` | How many apples the batch button drops. |

If the number of prizes doesn't match the board, or a wall or missing peg is out of range, the page shows a message saying what to fix.

### The three start modes

```js
// 1. Center start (the November board)
chutes: [ { name: "Center", offset: 0 } ],
startMode: "fixed", fixedChute: "Center",

// 2. Fixed off-center chute (other chutes are drawn closed)
chutes: [ { name: "Left", offset: -1 }, { name: "Center", offset: 0 }, { name: "Right", offset: 1 } ],
startMode: "fixed", fixedChute: "Left",

// 3. Player's choice (the log gains a "Chute" column)
chutes: [ { name: "Left", offset: -1 }, { name: "Center", offset: 0 }, { name: "Right", offset: 1 } ],
startMode: "choice", fixedChute: "Center",   // fixedChute = default selection
```

Adding chutes widens the board, which adds slots, so update `slots` to match. The page tells you how many it needs.

## Guardrails (please keep these)

- The page never shows or computes per-slot chances, a theoretical distribution, or an average payout.
- There is no answer key in this repo. Work it out offline, and check it again whenever you change the config.
- There is no hidden mode or URL parameter. Everything is in plain sight in `index.html`.
- There is no automatic tally, histogram, or running average. The log is a plain list, so the summarizing and modeling stay with the teams.
