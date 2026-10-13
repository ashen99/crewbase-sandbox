# App comparison: Obsidian, Logseq, Joplin

_Prepared by writer on 2026-10-13 from the "apps" table (scout research, checker verification). Tracking issue: #1._

Scope: note-taking apps. All three have a free core app. The comparison focuses on the optional paid sync/cloud tiers, plus open-source status.

## Comparison

| Rank (cheapest paid tier first) | App | Core app | Cheapest paid tier | Other paid tiers | Open source | Verified |
|---|---|---|---|---|---|---|
| 1 | Joplin | Free | EUR 2.99/month Basic Cloud (EUR 2.40/mo, EUR 28.69/yr) | Pro EUR 5.99/mo (EUR 57.48/yr); Teams EUR 6.69/user/mo, min 2 users (EUR 80.28/yr) | Yes (MIT) | Yes (checker, joplinapp.org/plans) |
| 2 | Logseq | Free, all core features | About $5/month Sync (about $50/yr) | Sponsor tier about $15/month | Yes (AGPL-3.0) | **No**: pricing page has no pricing content |
| 3 | Obsidian | Free for personal use | $5/user/month Sync ($4 billed annually) | Publish $10/site/mo ($8 annual); Catalyst $25 one-time; Commercial $50/user/yr | No (closed source) | Yes for pricing (obsidian.md/pricing); closed source not checked on page |

Ranking method: `rank.py` sorts by the cheapest optional paid tier, using the list monthly price. Obsidian and Logseq are in USD and Joplin is in EUR. No currency conversion was applied, so the ranking is approximate. Joplin's EUR price is the lowest number, but the gap is small once currency is considered.

## Caveats
- **Logseq's prices (Sync about $5/mo, Sponsor about $15/mo) and its open-source claim are unverified.** The official pricing page loaded but showed no prices. Logseq's position in the ranking depends on these figures.
- Obsidian's monthly Sync/Publish prices ($5/$10) are not visible in the default pricing view on the page. The annual prices are confirmed.
- Joplin's Teams monthly price was removed because the page shows only the per-user annual-equivalent price.

## Recommendation
- **For the lowest-cost cloud sync:** Joplin. It is free and open source (MIT), and its cheapest cloud tier is the lowest priced of the three, at about EUR 2.40 to 2.99 per month.
- **For a closed-source, polished option with a free personal tier:** Obsidian. Sync costs $4 to $5 per month. Choose it if the closed source is acceptable.
- **For open source with optional sync:** Logseq is a reasonable option, but do not rely on its pricing until it is verified against another source.
- If you do not need paid sync, all three are free to use. Pick based on the editing model and open-source needs.

## Under the hood

Architecture descriptions from DeepWiki (one question each). These were not independently verified against the source code.

**Logseq (logseq/logseq):** Logseq uses a multi-process architecture. The browser renderer (React/Rum UI) talks to a DB worker thread via Comlink RPC over postMessage. The worker runs graph transactions and queries, with DataScript as the in-memory database and SQLite for persistence (OPFS in the browser, filesystem SQLite on desktop and CLI via a `db-worker-node` daemon). A `db-sync` worker (D1/Durable Objects SQLite and R2) handles remote graph sync. The core logic is written in ClojureScript.

**Joplin (laurent22/joplin):** Joplin is an offline-first monorepo. The desktop (Electron/React), mobile (React Native) and CLI apps share `@joplin/lib`, which holds the business logic, data models, `Synchronizer` and plugin system. Each device keeps a local SQLite database. Sync targets include Joplin Cloud, WebDAV, S3, Dropbox, OneDrive and the local filesystem, with optional end-to-end encryption (RSA). Joplin Server is a Node.js REST API that typically uses PostgreSQL and S3.
