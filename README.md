## Baran Kaya

I'm 21, studying International Relations in Istanbul, and I have no formal software training. I teach English for the PTE Academic exam, and over the past year I've built and now run the entire technical side of that business myself — plus a video publishing pipeline that serves tens of millions of views a month.

Most of what I've built is private, because it *is* the business. This page is here so the work is at least legible from the outside.

---

### Publishing infrastructure

A Cloudflare Workers + KV pipeline that renders, uploads, schedules and publishes short-form video to three Instagram accounts on a 3-minute cron, entirely on free-tier infrastructure. It runs unattended and reports to me over Telegram when something breaks.

Live numbers, pulled from the Meta Graph API on **21 August 2026**:

| Account | Followers | Posts published | Views (last 30 days) |
|---|---:|---:|---:|
| [@thatindiearchive](https://instagram.com/thatindiearchive) | 39,527 | 293 | 29,288,548 |
| [@turkuarsivinden](https://instagram.com/turkuarsivinden) | 36,454 | 310 | 28,409,029 |
| [@thatmelancholyarchive](https://instagram.com/thatmelancholyarchive) | 8,655 | 180 | 7,334,804 |
| **Total** | **84,636** | **783** | **65,032,381** |

What's actually involved: `ffmpeg` render pipeline (1080×1920, letterbox detection, typeset hooks), a single Worker resolving per-account Meta page tokens from secrets, video bytes stored in KV because R2 costs money, a serial job state machine (`pending → processing → published | failed`) with a 20-minute stuck-job guard, and daily analytics snapshots written back to KV.

The interesting part isn't the happy path — it's the failure modes, which I learned by losing production data. Instagram fingerprints video **perceptually**, so the same file will never publish twice, and a re-encode with a brightness nudge doesn't fool it. One stuck `processing` job blocks the entire queue. KV's free tier gives you 1,000 writes a day, and a worker that rewrites its whole schedule on every tick will eat 640 of them before you notice. I've written all of this down.

→ [**reels-scheduler**](https://github.com/barankaya7/reels-scheduler) — the scheduler, extracted and documented.

---

### Exam-prep platform

**`pte-services`** — 19,700 lines, 87 commits. Next.js 16 + Prisma + Supabase. The CRM and course platform behind my teaching business: student and lesson management, payments, resource delivery, a booking system with Google Calendar integration, UTM and ad-spend attribution, WhatsApp Business webhooks, and thirteen scheduled jobs handling lesson creation, reminders, inactivity checks and follow-ups.

The **Claude API** does the language work — writing assessment and the conversational follow-up agent — alongside Azure Pronunciation Assessment and Whisper for speaking evaluation. I'm an Anthropic API customer, not just a Claude Code user.

**`pte-workshop`** — 17,600 lines. The student-facing course platform: 13-question placement test with skill-gap analysis, AI question generation, assignment submission and grading, progress dashboards, leaderboards.

**`pte-app`** — Expo / React Native client for the same platform.

**`trace-cockpit`** — a live-lesson console for online teaching. The problem it solves is specific: when you screen-share an exam paper on Google Meet, you cannot look at the answer key. So it splits into a private teacher cockpit (answer keys, timers, controls) and a screen-shared student stage, kept in sync, plus a homework system where answers are graded server-side and never reach the client before submission.

---

### Other

**`persona-forge`** — distills a body of video content into a topic-tagged knowledge base, then answers a single question from every distilled perspective at once, reporting disagreement rather than averaging it away.

**`sp500-tracker`** — S&P 500 dashboard combining price and fundamentals with SEC Form 4 insider filings, 13F institutional holdings and congressional trades into a daily composite score. Currently shelved.

---

### A note on the private repos

Seven of my repositories are private because they're the code my income depends on, and I don't have merged pull requests to other people's projects — I've been busy shipping my own. If you're evaluating me for something and need to see the actual work, ask and I'll grant read access.

📧 barankaya1502@gmail.com
