# LifeOS — Your Personal Decision & Follow-Through Engine

> A calm, intelligent layer over your daily life. Not another app to manage — an AI that understands your context, helps you decide, and makes sure you follow through.

---

## The Problem

People don't struggle with *information*. They struggle with:

- **What to prioritize** — too many options, no clear signal
- **Staying consistent** — motivation fades, systems break down
- **Remembering everything** — context gets lost across apps
- **Making good decisions repeatedly** — judgment degrades under load

Current tools (ChatGPT, Notion, Todoist) help *in moments*. But no system connects your:

```
Intentions → Decisions → Actions → Outcomes
```

LifeOS does.

---

## What LifeOS Does

### 1. Daily Command Center
Every morning, automatically generates:
- Your top 3 priorities for the day
- A suggested schedule based on your real calendar + habits
- Proactive warnings ("You're overbooked today", "You haven't slept enough")

### 2. Smart Decision Engine
Instead of generic answers, context-aware ones:

| Ask | Get |
|-----|-----|
| "What should I eat?" | "Based on your budget, goals, and what you ate yesterday — here's the best option." |
| "Should I take this meeting?" | "You have 3 focus blocks today. This conflicts with your deep work window." |
| "Buy this $200 gadget?" | "You've spent $340 on non-essentials this week. Your budget target is $200/week." |

### 3. Follow-Through System *(the killer feature)*
Most apps track. LifeOS adapts.

- Notices when you're procrastinating
- Adjusts plans in real time
- Nudges intelligently — not spammy

```
"You skipped the gym twice this week. Want a 20-minute workout today instead?"
```

### 4. Passive Life Tracking
Quietly pulls from your existing apps:
- 📅 Calendar
- 📧 Email
- 💬 Messages
- 📱 Apps you already use

No manual logging. No new habits to build.

### 5. Relationship Intelligence
Remembers who matters and when you last connected:

```
"You haven't talked to Jake in 3 weeks — check in?"
"Sarah's birthday is in 4 days."
```

### 6. Money + Time Awareness
Connects behavior to consequences — then guides:

```
"You spent $120 eating out this week."
"You lose ~10 hours/week scrolling at night."
→ Here's how to reclaim both.
```

---

## Tech Stack (MVP)

```
Frontend:     React + TypeScript + Tailwind CSS
Backend:      Node.js + Express
AI Layer:     OpenAI API (GPT-4o)
Database:     Supabase (PostgreSQL + Auth)
Integrations: Google Calendar API, Gmail API
Hosting:      Vercel (frontend) + Railway (backend)
```

---

## Project Structure

```
lifeos/
├── src/
│   ├── components/         # Reusable UI components
│   │   ├── CommandCenter/  # Daily briefing UI
│   │   ├── DecisionEngine/ # Decision input/output
│   │   ├── FollowThrough/  # Nudge & tracking UI
│   │   └── Dashboard/      # Main overview
│   ├── hooks/              # Custom React hooks
│   │   ├── useLifeContext.ts
│   │   ├── useDecision.ts
│   │   └── useNudge.ts
│   ├── lib/                # Core utilities
│   │   ├── ai.ts           # OpenAI wrapper
│   │   ├── calendar.ts     # Google Calendar integration
│   │   ├── memory.ts       # User context/memory store
│   │   └── nudge.ts        # Follow-through engine
│   ├── pages/              # App routes
│   │   ├── index.tsx       # Daily briefing
│   │   ├── decide.tsx      # Decision engine
│   │   └── review.tsx      # Weekly review
│   └── styles/
├── docs/                   # Architecture docs
├── .env.example
├── package.json
└── README.md
```

---

## Getting Started

### Prerequisites
- Node.js 18+
- OpenAI API key
- Google Cloud project (for Calendar/Gmail APIs)
- Supabase account

### Installation

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/lifeos.git
cd lifeos

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Fill in your API keys (see .env.example)

# Start development server
npm run dev
```

### Environment Variables

```env
# .env.example
OPENAI_API_KEY=your_openai_key_here
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_supabase_anon_key
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## Core Architecture

### The Memory Layer

LifeOS maintains a persistent user context object that gets updated continuously:

```typescript
interface UserContext {
  goals: Goal[]
  habits: Habit[]
  preferences: Preferences
  recentDecisions: Decision[]
  relationships: Contact[]
  calendarEvents: CalendarEvent[]
  financialContext: FinancialSnapshot
  energyPatterns: EnergyPattern[]
}
```

### The Decision Engine

Every decision query runs through a 3-step pipeline:

```
1. Context Fetch    → Pull relevant memory slices
2. AI Reasoning     → Generate options with tradeoffs
3. Personalization  → Filter through user values/goals
```

### The Follow-Through Engine

```typescript
// Runs on a cron schedule, checks for missed commitments
async function checkFollowThrough(userId: string) {
  const context = await getUserContext(userId)
  const missedCommitments = findMissed(context.commitments)
  
  if (missedCommitments.length > 0) {
    const nudge = await generateAdaptiveNudge({
      missed: missedCommitments,
      userEnergy: context.currentEnergyLevel,
      timeAvailable: context.nextFreeSlot
    })
    
    await sendNudge(userId, nudge)
  }
}
```

---

## Roadmap

### v0.1 — MVP *(current focus)*
- [ ] Daily briefing generator
- [ ] Basic memory store (goals, habits, preferences)
- [ ] Simple follow-up nudge system
- [ ] Google Calendar integration

### v0.2 — Decision Engine
- [ ] Context-aware decision answering
- [ ] Decision history + pattern learning
- [ ] Tradeoff visualization

### v0.3 — Proactive Intelligence
- [ ] Passive tracking integrations (email, messages)
- [ ] Relationship intelligence
- [ ] Money + time awareness reports

### v1.0 — Full LifeOS
- [ ] Mobile app (React Native)
- [ ] Wearable data integration
- [ ] Team/shared context mode

---

## Design Principles

1. **Proactive over reactive** — LifeOS sees, suggests, adapts. Not just answers.
2. **No new habits required** — Pull from existing tools. Zero manual logging.
3. **Guidance over reporting** — Don't just tell people what happened. Tell them what to do.
4. **Trust through transparency** — Always explain why a suggestion is made.
5. **Calm by default** — Nudges should feel like a thoughtful friend, not a notification machine.

---

## Monetization

| Tier | Price | Features |
|------|-------|----------|
| Free | $0/mo | Daily briefing, basic decisions, 1 integration |
| Pro | $15/mo | Full decision engine, all integrations, follow-through system |
| High Performance | $25/mo | Proactive AI mode, relationship intelligence, advanced analytics |

---

## Contributing

This project is in early development. Contributions welcome!

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/decision-engine`)
3. Commit your changes
4. Open a pull request

See [CONTRIBUTING.md](docs/CONTRIBUTING.md) for guidelines.

---

## Honest Reality Check

This is a strong concept — but:

- **Execution > concept.** The idea isn't the hard part.
- **UX will make or break it.** If it feels like work to use, people won't.
- **Trust is everything.** People won't use this if it feels invasive or wrong.
- **Start narrow.** One great feature beats five mediocre ones.

The initial target: people who *want* to improve their lives but struggle with consistency. Young professionals, entrepreneurs, goal-oriented people aged 22–35.

---

## License

MIT — see [LICENSE](LICENSE)

---

*Built with the belief that AI's highest use isn't answering questions — it's helping people become who they're trying to be.*
