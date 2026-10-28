# Ziki Improvement: Agent Fleet Rate-Budget & Window Countdown Dashboard

[![Ziki Platform](https://img.shields.io/badge/Platform-Ziki.dev-14b8a6?style=for-the-badge&logo=codeforces&logoColor=white)](https://ziki.dev)
[![HNG Internship](https://img.shields.io/badge/HNG-Internship-blue?style=for-the-badge)](https://hng.tech)
[![Stack](https://img.shields.io/badge/Tech_Stack-HTML5_•_Vanilla_CSS_•_Modern_JS-f97316?style=for-the-badge)](https://developer.mozilla.org)

An interactive, production-grade frontend solution addressing the **"Invisible Rate-Limit Cliff"** on the [Ziki autonomous issue tracking and confirmation platform](https://ziki.dev).

---

## 📌 The Problem: The Invisible Rate-Limit Cliff

During extensive automated issue review workflows on Ziki (`project_zedu`), human owners and autonomous agents frequently experience severe interaction friction:

1. **The Silent Rate-Limit Wall**: Standard reviewer agents are provisioned with **30 writes/hour** (1 write every 2 minutes). When batch validations exhaust this budget in ~2 minutes, the agent abruptly hits an `HTTP 429 Too Many Requests` and must sleep for up to 58 minutes.
2. **Zero In-App Quota Visibility**: Neither the Ziki dashboard (`/settings/agents`) nor standard API queries (`GET /api/v1/agents/me`) expose write quota counters, current consumption, or time remaining until reset. To human owners, the agent simply appears frozen or crashed.
3. **The Hidden Permission Gate**: High-Volume mode (400 writes/hr + 4,000 reads/hr) provides a **13.3x speedup** (slashing a 7-hour confirmation backlog to ~30 minutes). However, it requires **Tenant Admin** authorization—a requirement neither explained on the pairing screen nor highlighted on the agent overview.

---

## 💡 The Solution: Real-Time Fleet Control Dashboard

This dashboard provides human operators and autonomous systems with complete real-time visibility and proactive control over their agent fleet's operating budget.

### Key Capabilities

- **⚡ Dual-Ring Master Radial SVG Gauge**:
  - **Inner Ring**: Real-time write quota remaining (dynamically color-coded: Cyan $\ge 50\%$, Amber $20\text{--}50\%$, Rose $\le 20\%$).
  - **Outer Ring**: Progress indicator for the active 60-minute rolling rate-limit window.
- **⏱️ Per-Agent Ticking Window Countdown**:
  - Live second-by-second countdown (e.g. `23m 34s`) with an animated SVG timer ring.
  - Automatically resets to full budget when the 1-hour window expires.
- **📊 Fleet Throughput Aggregation**:
  - Real-time fleet write capacity (e.g. `150 / hr` standard vs `2,000 / hr` high-volume).
  - Remaining writes across the entire pool.
  - Earliest upcoming reset timestamp.
  - Verified points tracker (**53,881+ points** from live confirmations).
- **🛡️ Graceful Throttle Visualization**:
  - Replaces vague "frozen" states with an explicit `⛔ Throttled (Sleeping until XX:XX)` state whenever rate limits are reached.
- **🚀 Interactive Admin High-Volume Simulator**:
  - Interactive authorization modal demonstrating the seamless elevation from Standard (30/hr) to High-Volume (400/hr).
  - Live write simulation (`-1 Write`) with immediate gauge updates and toast notifications.
  - Force 429 Throttle simulation to test error handling states.

---

## 🛠️ Technology Stack

Built strictly following web standards with zero bloated dependencies:

- **HTML5**: Semantic markup, accessible labels, structured hierarchy.
- **Vanilla CSS3**: Modern design system using CSS custom properties (variables), responsive CSS Grid, Flexbox, glassmorphism, and hardware-accelerated animations.
- **Vanilla ES6+ JavaScript**: Lightweight, event-driven reactive state management with live ticking timers and SVG path math.
- **Typography**: Google Fonts (`Outfit`, `Inter`, and `JetBrains Mono`).

---

## 📋 Recommended Backend API Schema Addition

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

## 🚀 Getting Started

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

## 👥 Real-World Context (Tested Agents)

The dashboard reflects real data and agent identities validated during autonomous review loops on Ziki:

| Agent Name | Role | ID | Base Budget |
|---|---|---|---|
| **Denise** | Reviewer | `cmvl4wzeq0e1qqv01hrg20dg0` | 30 / hr |
| **Deni** | Reviewer | `cmvl4yart0ef3qv01j4dozjw8` | 30 / hr |
| **Royto** | Reviewer | `cmvl4zd1p0equqv01xvml7qd9` | 30 / hr |
| **Codex** | Reviewer | `cmvhxelxq0fn4pkumv2nogtrr` | 30 / hr |
| **Zedu Triager 1** | Reviewer | `cmvk17r6p1d73ns01odzuukpk` | 30 / hr |

---

## 📄 License
MIT License. Created as part of product exploration and UI/UX improvement on Ziki.
