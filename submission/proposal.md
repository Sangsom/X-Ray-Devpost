# Portfolio Tracker: X-Ray

## Summary
Portfolio Tracker: X-Ray is a manual-first, multi-asset portfolio tracker for DIY investors. It solves one problem: “My investments are spread everywhere, and I can’t see the full picture.”  
In under 30 seconds, users can see their net worth, allocation, and overexposure risks across asset types. Premium unlocks deeper risk and diversification insights.

---

## The Problem (messy reality)
Modern investors rarely use one platform:
- stocks/ETFs in a broker
- funds in another account
- gold in a physical form or separate service
- real estate and private assets with no live data
- liabilities like mortgages that affect true net worth

This creates a few common pain points:
1. No single view of net worth and allocation  
2. Risk is hidden (country/sector concentration, “too much in one thing”)  
3. Non-listed assets go stale because people forget to update values  
4. Existing tools often require bank linking, don’t support non-listed assets well, or feel like spreadsheets

X-Ray is built around a simple idea: manual entry is not a weakness. It’s how you include the full portfolio, including real-world assets that bank-linked apps miss.

---

## Target Audience
**Primary audience:** DIY investors with diversified portfolios  
- People who invest across multiple platforms and asset types
- Users who value clarity and control over “auto-link everything”
- Investors who care about diversification and risk (not day trading)

**Typical user situation**
- “I have stocks + ETFs + a fund + some gold + a mortgage + maybe a property.”
- “I want one clean picture and a sanity check on exposure.”

---

## Why X-Ray is better (and different)
X-Ray is not a trading app and not a robo-advisor. It’s a clarity tool.

It’s designed for fast understanding:
- Manual-first logging (fast, forgiving)
- Clear dashboard (“I get it” screen)
- Exposure visualizations (country + sector)
- Premium insights that explain risk in normal language

It also handles non-listed assets properly:
- manual valuations
- reminders to update values or payment milestones

---

## Core MVP Experience
1. Onboarding: user understands the promise quickly (“everything you own, one picture”).
2. Add assets (or load demo portfolio): stocks/ETFs, funds, gold, real estate, fixed income, custom.
3. Dashboard “aha”:
   - net worth (assets minus liabilities)
   - allocation by asset type
   - exposure by country and sector
   - overexposure warning banner when thresholds are crossed
4. Insights:
   - Free: Data Confidence (how complete your country/sector tagging is)
   - Premium: risk and diversification insights with drill-down detail

---

## Monetization Strategy (RevenueCat subscription)
**Model:** Subscription only (monthly + annual), implemented via RevenueCat.

### Free vs Premium boundary (clear and honest)
**Free includes**
- tracking and managing assets/liabilities
- dashboard (net worth, allocation, exposure)
- basic signals and data confidence

**Premium unlocks**
- detailed risk/diversification insights and breakdowns:
  - Concentration Score (how dominated the portfolio is)
  - Home Bias (overweight in home country)
  - Illiquidity Ratio (how much is hard to exit quickly)
  - Exposure Warnings (threshold-based risk flags)
- drill-down screens with “Why it matters” + “What to do next”

### Why subscription makes sense
- Investors keep portfolios for years; insights remain valuable over time
- Premium is not “pay to use the app”, it’s “pay for serious analysis”

### Paywall positioning (not pushy)
- Premium is presented when users tap locked insight details
- Copy focuses on value: “Understand concentration and overexposure”
- Restore purchases supported and visible

---

## What comes next (after MVP)
- Better tag suggestions (country/sector) for known symbols
- More pricing/metadata coverage and additional providers if needed
- Smarter reminders (valuation schedules for non-listed assets)
- Export options (PDF/CSV) and share improvements
- Optional import helpers (but only if they don’t harm simplicity)

---

## One sentence pitch (for judges)
A manual-first portfolio tracker that turns fragmented holdings into one clean picture, and unlocks premium risk + diversification insights that investors actually understand.
