# Ziki Improvement: Agent Fleet Rate-Budget & Window Countdown Dashboard

[![Ziki Platform](https://img.shields.io/badge/Platform-Ziki.dev-14b8a6?style=for-the-badge&logo=codeforces&logoColor=white)](https://ziki.dev)
[![HNG Internship](https://img.shields.io/badge/HNG-Internship-blue?style=for-the-badge)](https://hng.tech)
[![Stack](https://img.shields.io/badge/Tech_Stack-HTML5_�_Vanilla_CSS_�_Modern_JS-f97316?style=for-the-badge)](https://developer.mozilla.org)

An interactive, production-grade frontend solution addressing the **"Invisible Rate-Limit Cliff"** and enhancing **Human-Agent Interaction (HAI)** on the [Ziki autonomous issue tracking and confirmation platform](https://ziki.dev).

---

## ?? The Problem: The Invisible Rate-Limit Cliff & Black-Box Execution

During extensive automated issue review workflows on Ziki (`project_zedu`), human owners and autonomous agents frequently experience severe interaction friction:

1. **The Silent Rate-Limit Wall**: Standard reviewer agents are provisioned with **30 writes/hour** (1 write every 2 minutes). When batch validations exhaust this budget in ~2 minutes, the agent abruptly hits an `HTTP 429 Too Many Requests` and must sleep for up to 58 minutes.
2. **Zero In-App Quota Visibility**: Neither the Ziki dashboard (`/settings/agents`) nor standard API queries (`GET /api/v1/agents/me`) expose write quota counters, current consumption, or time remaining until reset. To human owners, the agent simply appears frozen or crashed.
3. **The Hidden Permission Gate**: High-Volume mode (400 writes/hr + 4,000 reads/hr) provides a **13.3x speedup** (slashing a 7-hour confirmation backlog to ~30 minutes). However, it requires **Tenant Admin** authorization�a requirement neither explained on the pairing screen nor highlighted on the agent overview.
4. **Cognitive Disconnect in Human-Agent Collaboration**: Operators have no instant way to observe which agent is actively reasoning, executing mutations, or waiting for human decisions.

---

## ?? The Solution: Real-Time Fleet Control Dashboard & Ziki's Pulse

This dashboard provides human operators and autonomous systems with complete real-time visibility, proactive control, and transparent human-agent interaction.

### Key Capabilities

- **? Ziki's Pulse (Dynamic HAI State Indicator)**:
  - Navbar cognitive status pill displaying the exact agent actively working in real-time (e.g. `Denise is ready`, `Denise is thinking...`, `Denise is searching...`, `Denise needs your call`).
  - Pulsing animated status dot color-coded to the operational state (Ready: Emerald, Thinking: Purple, Busy/Writing: Blue, Waiting/Throttled: Amber).
  - Expandable telemetry card providing live drill-downs: **Current Focus**, **Agent at Work**, **Fleet Health (Active vs. Sleeping)**, and **Session Timestamp**.
- **? Dual-Ring Master Radial SVG Gauge**:
  - **Inner Ring**: Real-time write quota remaining (dynamically color-coded: Cyan $\ge 50\%$, Amber $20\text{--}50\%$, Rose $\le 20\%$).
  - **Outer Ring**: Progress indicator for the active 60-minute rolling rate-limit window.
- **?? Per-Agent Ticking Window Countdown**:
  - Live second-by-second countdown (e.g. `23m 34s`) with an animated SVG timer ring.
  - Automatically resets to full budget when the 1-hour window expires.
- **?? Streamlined Above-the-Fold Fleet Summary**:
  - Clean, compact metrics strip surfacing **Fleet Remaining Writes**, **Earliest Pool Reset**, and **Total Fleet Capacity** without visual clutter.
- **??? Graceful Throttle Visualization**:
  - Replaces vague "frozen" states with an explicit `? Throttled (Sleeping until XX:XX)` state whenever rate limits are reached.
- **?? Interactive Admin High-Volume Simulator**:
  - Interactive authorization modal demonstrating the seamless elevation from Standard (30/hr) to High-Volume (400/hr).
  - Live write simulation (`-1 Write`) with immediate gauge updates and toast notifications.
  - Force 429 Throttle simulation to test error handling states.

---

## ?? Recent Improvements & Changelog

### 1. Human-Agent Interaction (HAI): "Ziki's Pulse"
- Added an interactive presence indicator directly into the top navigation.
- Tailored state labels to dynamically reflect the **active agent's name** instead of a generic bot identifier (e.g., *Codex is thinking...*, *Royto is searching...*).
- Interactive triggers linked across agent selection, mutation simulation, and rate-limit throttle events.

### 2. UI Simplification & Spatial Optimization
- Eliminated redundant page header banners and duplicate static status badges.
- Consolidated oversized metric cards into a clean, horizontal fleet status strip.
- Tightened vertical spacing across the layout to ensure both the reviewer agent grid and inspector deep-dive panel sit comfortably above the fold.

---

## ??? Technology Stack

Built strictly following web standards with zero bloated dependencies:

- **HTML5**: Semantic markup, accessible labels, structured hierarchy.
- **Vanilla CSS3**: Modern design system using CSS custom properties (variables), responsive CSS Grid, Flexbox, glassmorphism, and hardware-accelerated animations.
- **Vanilla ES6+ JavaScript**: Lightweight, event-driven reactive state management with live ticking timers and SVG path math.
- **Typography**: Google Fonts (`Outfit`, `Inter`, and `JetBrains Mono`).

---

## ?? Recommended Backend API Schema Addition

To natively support this UI, the following schema enhancement is proposed for `GET /api/v1/agents/me`:

```json
{
  "id": "cmvl4wzeq0e1qqv01hrg20dg0",
  "name": "Denise",
  "level": "reviewer",
  "tier": "standard",
  "budgets": {
    "write": {
      "limit": 30,
      "remaining": 18,
      "resets_in_seconds": 1420,
      "window_seconds": 3600
    },
    "read": {
      "limit": 600,
      "remaining": 540,
      "resets_in_seconds": 1420,
      "burst_limit": 4000
    }
  }
}
```

---

## ?? Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/Akinbewaji/Ziki-improvement.git
cd Ziki-improvement
```

### 2. Run Locally
Open `index.html` in any modern web browser:
```bash
# On Windows
start index.html

# On macOS
open index.html

# On Linux
xdg-open index.html
```

Or serve via any static HTTP server (e.g. `npx serve .` or VS Code Live Server).

---

## ?? Real-World Context (Tested Agents)

The dashboard reflects real data and agent identities validated during autonomous review loops on Ziki:

| Agent Name | Role | ID | Base Budget |
|---|---|---|---|
| **Denise** | Reviewer | `cmvl4wzeq0e1qqv01hrg20dg0` | 30 / hr |
| **Deni** | Reviewer | `cmvl4yart0ef3qv01j4dozjw8` | 30 / hr |
| **Royto** | Reviewer | `cmvl4zd1p0equqv01xvml7qd9` | 30 / hr |
| **Codex** | Reviewer | `cmvhxelxq0fn4pkumv2nogtrr` | 30 / hr |
| **Zedu Triager 1** | Reviewer | `cmvk17r6p1d73ns01odzuukpk` | 30 / hr |

---

## ?? License
MIT License. Created as part of product exploration and UI/UX improvement on Ziki.
