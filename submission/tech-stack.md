# Technical Documentation — Portfolio Tracker: X-Ray

This document describes the tech stack, architecture, and RevenueCat implementation for Portfolio Tracker: X-Ray (iOS).

---

## 1) Tech Stack

- Platform: iOS
- UI: SwiftUI
- Navigation: TabView + per-tab NavigationStack
- Persistence: SwiftData (local-first, offline-capable)
- Live pricing:
  - Provider abstraction (`LivePriceProvider`)
  - Current vendor: Marketstack
  - Caching + “last updated” metadata
- Currency conversion:
  - Base currency setting
  - FX rates provider + “FX Rates” screen for transparency
- Subscriptions: RevenueCat (monthly + annual)
- Analytics: Firebase Analytics (optional, user-controlled)
- Notifications: Local notifications for reminders (opt-in)

---

## 2) High-Level Architecture

The app is split into UI, domain logic, and services. The goal is testable business logic and swappable integrations.

### Modules / layers (conceptual)
- UI (SwiftUI Views)
- ViewModels (state + orchestration)
- Domain models (Asset, Position, Valuation, Liability, Reminder)
- Services
  - PortfolioSnapshotBuilder (normalization)
  - InsightsEngine (pure calculations)
  - PriceService (orchestration + caching + staleness)
  - FXService (conversion)
  - AnalyticsService (events)
  - RevenueCatPremiumAccessProvider (entitlement status)

### Navigation
- Root: TabView
  - Dashboard tab (NavigationStack)
  - Assets tab (NavigationStack)
  - Insights tab (NavigationStack)
  - Settings tab (NavigationStack)

Deep linking is supported (e.g., Dashboard overexposure banner → Insights and highlight).

---

## 3) Data Model (Core Entities)

### Asset
Represents one “thing you own” (or track):
- type: stocks/ETFs, funds, gold, fixed income, real estate, custom
- name
- currency
- tags (country, sector, symbol, etc.)
- optional purchase date
- pricing mode metadata (live/manual override)

### Position
Represents a holding quantity and cost basis (optional):
- quantity (e.g., shares)
- cost basis (optional)
- linked to Asset

### Valuation
Represents the asset’s value at a point in time:
- source: live or manual
- timestamp
- valueTotal (total value in asset currency)

### Liability
Represents debts that affect net worth:
- type (mortgage/loan/credit card)
- balance
- optional linked asset (e.g., mortgage linked to real estate)
- currency

### Reminder
Represents “update value” or “payment milestone” reminders:
- type
- next due date
- notification scheduling (local notifications)

---

## 4) Portfolio Calculation Pipeline

All dashboard totals and insights are calculated from a normalized snapshot.

### Snapshot creation
Portfolio data (Assets + Positions + Valuations + Liabilities)
→ `PortfolioSnapshotBuilder.build()`
→ `PortfolioSnapshot`

Snapshot responsibilities:
- determine base currency (from user Settings)
- convert values to base currency using FX rates
- select the “active value” per asset (manual override > latest live > latest manual)
- prepare tag extraction (country/sector/symbol)
- track coverage and excluded items (e.g., missing FX)

### Dashboard reads from snapshot
- Net worth = totalAssets - totalLiabilities
- Allocation = grouped by asset type (collapsed “Other”)
- Exposure = grouped by country or sector (top categories + “Other” + “Unknown”)

Staleness
- Staleness is shown only when live pricing is in use and last update exceeds threshold.

---

## 5) Insights Architecture

PortfolioSnapshot
→ InsightsEngine (pure functions)
→ [InsightCardModel]
→ InsightsViewModel
→ Insights UI (cards + drill-down detail)

### Insights included
- Data Confidence (Free)
- Concentration Score (Premium)
- Home Bias (Premium)
- Illiquidity Ratio (Premium)
- Exposure Warnings (Premium)

The InsightsEngine produces UI-ready insight models with:
- title
- 1-line summary
- severity (Low/Medium/High)
- key metric + breakdown list
- “Why it matters” + “What to do next”

Math note (Concentration)
We use a Herfindahl–Hirschman style index:
HHI = Σ(w_i^2), where w_i = V_i / totalValue
Score = 100 * HHI

The goal is explainable risk detection, not financial advice.

---

## 6) Live Pricing (Marketstack) + Caching

### Provider abstraction
`LivePriceProvider` defines:
- fetchQuote(symbol) → Quote (price, timestamp, etc.)
- typed errors: missingApiKey, unauthorized, rateLimited, networkFailure, decodeFailure

### PriceService (orchestration)
- determines which assets are live-eligible (has quote symbol)
- rate limiting behavior (avoid “refresh spam”)
- caching:
  - stores last successful update timestamp
  - exposes “last updated” to UI
- graceful failure:
  - never shows raw API errors to user
  - UI displays stale state and remains usable

---

## 7) RevenueCat Implementation (Subscriptions)

### Products
- Monthly subscription (Premium)
- Annual subscription (Premium)
Products and offerings are configured in RevenueCat and App Store Connect.

### Entitlement gating
- Entitlement: `premium`
- A single premium access provider exposes current status:
  - `isPremium == true` enables premium insight details
  - `isPremium == false` shows locked state and routes to paywall

### Paywall behavior
Entry points:
- Tapping locked insight card → opens paywall
- Tapping locked insight detail → opens paywall

Requirements:
- Purchase unlocks premium immediately
- Restore purchases supported
- Offline state:
  - paywall shows a calm “connect to internet” message
  - app remains usable (free features work)

### Testing (TestFlight / Sandbox)
- Use TestFlight build with external testing
- Confirm offerings load in production-like environment
- Validate:
  - purchase flow
  - entitlement updates
  - restore purchases
  - “already subscribed” behavior

---

## 8) Analytics (Optional)

Firebase Analytics is used to understand product usage and improve the experience.
- User can disable analytics at any time in Settings
- We do not send portfolio values or holdings content as analytics payloads
- Core events tracked:
  - onboarding started/completed
  - demo portfolio loaded
  - asset added/edited
  - insights viewed
  - paywall shown
  - purchase completed/restored

---

## 9) Reliability & UX Principles

- Local-first: portfolio works offline
- Graceful degradation:
  - pricing failures show stale state (not errors)
  - missing tags degrade insights transparently (Data Confidence)
  - missing FX rates show clear disclosure + “FX Rates” screen
- No “spreadsheet energy”: fast comprehension over maximum configuration

---

## 10) Support / Contact
For technical questions during judging/testing, provide:
- [TestFlight link](https://testflight.apple.com/join/nRCGTW8T)
- Basic “What to test” steps
- Support email (replace this in final docs): rinalds.domanovs@gmail.com
