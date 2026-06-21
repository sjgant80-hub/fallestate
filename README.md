# ◊ FallEstate

**Sovereign UK estate + letting agent property management. One HTML file. Zero dependencies.**

Prime 883 · v1.0.0 · part of the `fall-estate` bundle (anchor)

---

## For UK estate / letting agents

You run a 1–10 person agency (ARLA / NAEA Propertymark, TPO or PRS member, CMP scheme member).

FallEstate gives you, in one HTML file you can open on any device, with all data staying on that device:

- **Multi-firm, multi-agent, multi-property** workspace
- **Sales pipeline**: listed → viewings → offers → SSTC → exchanged → completed, with Memorandum of Sale generation
- **Lettings pipeline**: listed → viewings → offers → holding deposit → references → signing → deposit protected → keys, with holding-deposit register and Tenant Fees Act 2019 cap enforcement (1 week max)
- **Tenancy management**: AST / non-AST / company let / HMO, rent schedule, deposit, scheme (TDS / DPS / mydeposits), prescribed-info clock
- **Inventory** room-by-room with condition log
- **Periodic inspections** (quarterly default) with next-due tracker
- **Compliance tracker**: Gas Safety (CP12) annual, EPC (minimum E), EICR every 5 years, Right-to-Rent (England) share-code verification, deposit 30-day clocks with amber/red flags
- **Section 21 pre-checks**: blocks issuance unless deposit protected, prescribed info served, gas served, EPC served, How to Rent served, EPC ≥ E. Notice text generated when allowed.
- **Section 8 grounds** (1, 2, 7, 8, 10, 11, 12, 13, 14, 17) with Form 3 text
- **Client Money Protection** ledger: rent in, deposit held, deposit forwarded to scheme, disbursement to landlord, contractor payment, agency fee — daily held-balance shown
- **Mansoor P3 audit chain** — every state change hash-chained, 6-year retention, JSON export
- **14-rule T0 Q&A** offline (Tenant Fees Act, deposit protection, S21 abolition status, S8 grounds, Right-to-Rent, ARLA code, CMP, holding-deposit triggers, EICR, Gas Safety, EPC/MEES, eviction process). T3 BYOK for Anthropic / Gemini / OpenAI / OpenRouter — never used unless you set a key.
- **Bundle mesh** (`fall-estate`) — boots `sync.request`, broadcasts `property.*`, `tenancy.*`, `clientMoney.entry`, `deposit.protected`, `viewing.scheduled`. Also speaks `fall-signal` (KONOMI) and `fall-client` (IFA shared client schema).
- **PWA** manifest + install
- **Mobile-first** responsive

Open `index.html`. First launch walks you through firm setup (one screen) and your first agent. Two demo properties (1 sale, 1 let with active tenancy and CMP entries) are seeded — overwrite or archive them.

The disclaimer at the top of every screen tells the truth: this is an operational tool, not a property portal integration, not Land Registry, not regulated legal advice.

---

## For developers / forkers

```
fallestate/
├── index.html      # single-file artifact (≈138 KB)
├── README.md       # this
├── LICENSE         # MIT
└── .nojekyll       # Pages serves raw
```

**Stack:** vanilla JS, vanilla CSS, IndexedDB primary / localStorage fallback. No build step, no npm, no framework, no CDN imports, no telemetry. Chrome 113+ primary target; modern Safari/Firefox work.

**IDB stores (10):** `firms · advisers · clients · properties · tenancies · inventories · inspections · clientMoney · audit · settings`.

**Schema:** conforms to `ESTATE-BUNDLE-SHARED-SCHEMA.md v1.0`. The shared `Client` shape extends with role-specific fields (`role`, `selfManaging`, `taxStatus`, `nrlScheme`, `gasCheckExpiry`, `epcExpiry`, `referencesObtained`, `rightToRentCheckedAt`, `guarantorId`). `Property` carries `listingType`, `status`, `marketing`, `vendor[]`, `landlord[]`, `viewings[]`, `offers[]`, `rent`, `tenancy`, `inventories[]`, `inspections[]`.

**BroadcastChannel meshes:**
- `fall-estate` — `property.listed/updated/withdrawn/sstc/completed`, `tenancy.created/renewed/ended`, `clientMoney.entry`, `deposit.protected`, `viewing.scheduled`, `sync.request`/`sync.snapshot`
- `fall-client` — IFA-bundle client/adviser/firm sync (so estate clients show up in `falladviser`, `fallpaper`, etc., if open in another tab)
- `fall-signal` — KONOMI ping/pong for cross-tool discovery

**KONOMI shim:** `window.KONOMI.fallestate = { version, prime, getState() }` so other tools can introspect.

**Constants:** `TOOLNAME='fallestate' · VERSION='1.0.0' · PRIME=883`.

**14-pt gate:** single HTML < 500 KB · IDB primary · KONOMI shim · fall-signal + fall-estate mesh · PWA manifest via data: URL · mobile-first · MIT · two-audience README · `.nojekyll` · informational disclaimer · audit chain on by default · oxblood/brass/cream/void aesthetic · forkable brand (firm `brandColor` override) · sovereign (no outbound calls without explicit BYOK).

**Audit chain:** Mansoor P3 extended — `{i, ts, tool, adviserId, propertyId, tenancyId, clientId, action, reasoning, configVersion, prevHash, docHash, payload}`. Append-only. Hash-chained. 6-year retention (ARLA scheme + GDPR). JSON export from Firm tab → "Export".

**Forking:** override `state.firm.brandColor` (settings) and CSS var `--ox` to rebrand. The oxblood/brass palette is the bundle convention.

**Validate:** open in Chrome; check `window.KONOMI.fallestate.getState()`; verify `state.audit.length > 0` after creating any property; confirm `BroadcastChannel('fall-estate')` ping in DevTools.

MIT licence — fork, adapt, ship.

---

> "FallEstate is a tool for UK estate agents and letting agents (ARLA / NAEA / TPO members). It assists with property listing, tenancy management, Client Money Protection tracking, and Tenancy Deposit Protection compliance. It is not a property portal integration or Land Registry filing system. Sovereign — client data never leaves the device."
