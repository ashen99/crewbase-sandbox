# App comparison: Obsidian, Logseq, Joplin

_Prepared by writer on 2026-10-13 from the "apps" table (scout research, checker verification). Updated 2026-10-13 to add the Free tier column and the ranking explanation. Tracking issue: #1._

Scope: note-taking apps. All three have a free core app. The comparison focuses on the optional paid sync/cloud tiers, plus open-source status.

## Comparison

| Rank (cheapest paid tier first) | App | Free tier | Cheapest paid tier | Other paid tiers | Open source | Verified |
|---|---|---|---|---|---|---|
| 1 | Joplin | Free and open source | EUR 2.99/month Basic Cloud (EUR 2.40/mo, EUR 28.69/yr) | Pro EUR 5.99/mo (EUR 57.48/yr); Teams EUR 6.69/user/mo, min 2 users (EUR 80.28/yr) | Yes (MIT) | Yes (checker, joplinapp.org/plans) |
| 2 | Logseq | Free, all core features | About $5/month Sync (about $50/yr) | Sponsor tier about $15/month | Yes (AGPL-3.0) | **No**: pricing page has no pricing content |
| 3 | Obsidian | Free for personal use | $5/user/month Sync ($4 billed annually) | Publish $10/site/mo ($8 annual); Catalyst $25 one-time; Commercial $50/user/yr | No (closed source; not checked on the pricing page) | Yes for pricing (obsidian.md/pricing); closed source not checked on page |

## How to read the ranking

**The ranking is by cheapest paid tier, not by total cost.** It is not a statement that Obsidian costs the most. Obsidian's core app is free for personal use, and so are Logseq's and Joplin's core apps. Obsidian ranks last only because its cheapest paid add-on (Sync, $5/user/month monthly, $4 billed annually) is priced above Joplin's EUR 2.99 and about equal to Logseq's optional Sync (about $5/month, unverified). Obsidian's commercial use needs a $50/user/year license, which is a separate cost to consider.

`rank.py` sorts by the list monthly price of each app's cheapest optional paid tier. Obsidian and Logseq are in USD and Joplin is in EUR. No currency conversion was applied, so the ranking is approximate. Joplin's EUR price is the lowest number, but the gap is small once currency is considered.

## Caveats
- **Logseq's prices (Sync about $5/mo, Sponsor about $15/mo) and its open-source claim are unverified.** The official pricing page loaded but showed no prices. Logseq's position in the ranking depends on these figures.
- Obsidian's monthly Sync/Publish prices ($5/$10) are not visible in the default pricing view on the page. The annual prices are confirmed.
- Obsidian's closed-source status was not checked on its pricing page.
- Joplin's Teams monthly price was removed because the page shows only the per-user annual-equivalent price.

## Recommendation
- **For the lowest-cost cloud sync:** Joplin. It is free and open source (MIT), and its cheapest cloud tier is the lowest priced of the three, at about EUR 2.40 to 2.99 per month.
- **For a closed-source, polished option with a free personal tier:** Obsidian. Sync costs $4 to $5 per month. Choose it if the closed source is acceptable. Budget the $50/user/year commercial license if you use it at work.
- **For open source with optional sync:** Logseq is a reasonable option, but do not rely on its pricing until it is verified against another source.
- If you do not need paid sync, all three are free to use. Pick based on the editing model and open-source needs.

## Under the hood

Architecture summaries from DeepWiki (one question each, asked 2026-10-13). These were not independently verified against the source code.

**Logseq (logseq/logseq):** Built mainly in ClojureScript with React for the UI. It uses a two-thread design: the main UI thread handles rendering and editor state, and a DB worker thread runs graph transactions and queries. The two talk over RPC on `postMessage`. The worker uses DataScript as the in-memory graph database and SQLite for persistence. Data is local-first: on desktop the SQLite database under `~/logseq/graphs` is the source of truth, and in the browser SQLite is stored via OPFS. Electron wraps the web app for desktop, Capacitor covers iOS and Android, and `shadow-cljs` is the build tool. Cloud sync goes through a separate `db-sync` service.

**Joplin (laurent22/joplin):** An offline-first monorepo (Yarn workspaces). The desktop app is Electron with React, the mobile app is React Native, and the CLI uses `terminal-kit`. They share a common TypeScript backend, `@joplin/lib`, which holds sync, encryption, import/export, the database models and business logic, plus the shared `@joplin/renderer` for Markdown. Each device keeps a local SQLite database. Sync is handled by a `Synchronizer` class that works through `SyncTarget` implementations and `FileApi` drivers. Changes upload within seconds, and the client polls for downloads. Deletions propagate to all devices.
