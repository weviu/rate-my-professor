# Turkish Rate My Professor — Project Plan

## Core Features

- **Professor profiles** — name, university, department, courses taught
- **Student ratings** — numeric scores (1–5) for categories:
  - Teaching quality
  - Difficulty
  - Clarity / communication
  - Attendance impact
  - Exam fairness
- **Written reviews** — free-text comments with date/semester info
- **Search & filter** — by professor name, university, department, city
- **University & department database** — Turkish universities (200+, state and foundation)
- **Upvote/downvote reviews** — helpfulness voting
- **Professor "tags"** — e.g., *Sınava göre not verir*, *Devam zorunlu*, *Proje ağırlıklı*

---

## Turkish-Specific Considerations

| Topic | Detail |
|---|---|
| **Language** | Full Turkish UI; Turkish character support (`ş ğ ü ö ç ı İ`) in DB and search |
| **Universities** | YÖK (Yükseköğretim Kurulu) lists all accredited universities — use their public data |
| **Academic titles** | Dr., Doç. Dr., Prof. Dr., Öğr. Gör., Arş. Gör. |
| **Semester system** | Güz (Fall) / Bahar (Spring) + Yaz (Summer) |
| **Anonymity** | Students must feel safe — no linking reviews to real identities |

---

## Data Model

```
User → Review → Professor → Department → University
```

- `University`: name, city, type (devlet/vakıf), YÖK ID
- `Professor`: name, title, university, departments, photo
- `Review`: rating fields, comment, semester, anonymous user ref, helpful_count
- `User`: email (hashed/private), verified status, review history

---

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Next.js (SSR for SEO on professor pages) |
| Backend | Node.js/Express or Django |
| Database | PostgreSQL |
| Search | Meilisearch (handles Turkish stemming) |
| Auth | Email-based (no social login to preserve anonymity) |
| Hosting | Vercel + Supabase, or Turkish VPS |

---

## Key Concerns

1. **Abuse prevention** — fake reviews; use rate limiting and email verification
2. **Defamation risk** — reviews about real people; build a moderation/reporting system
3. **Seeding data** — site is useless empty; scrape or manually seed from YÖK's public professor database
4. **Turkish full-text search** — Turkish is agglutinative; a basic `LIKE` query won't work — use Meilisearch or PostgreSQL full-text search with Turkish config
5. **Mobile-first** — Turkish students primarily use mobile

