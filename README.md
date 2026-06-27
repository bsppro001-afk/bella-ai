# Bella — AI Personal OS for Students

An AI-powered personal operating system that helps students plan, track, learn,
and improve every day through personalised insights connected to Google apps.

---

## What's included

- **Auth** — Google OAuth + Email/password login (NextAuth)
- **Database** — PostgreSQL via Prisma ORM (users, tasks, habits, insights, chat)
- **AI Features** — All powered by Claude (claude-sonnet-4-6):
  1. Daily AI Study Plan
  2. Ask Bella Chatbot
  3. Smart Task Prioritisation
  4. Learning Insights & Gaps
  5. Habit & Streak Tracker
- **Google Integration** — Gmail, Calendar, Drive, Tasks
- **Frontend** — Next.js + React dashboard

---

## Setup Guide (step by step)

### Step 1 — Get your API keys

You need 4 things before deploying:

**A. Free PostgreSQL database**
- Go to https://neon.tech → Sign up free → Create project → Copy the connection string
- It looks like: `postgresql://user:pass@host/dbname?sslmode=require`

**B. Google OAuth credentials**
1. Go to https://console.cloud.google.com
2. Create a new project (name it "Bella")
3. Go to APIs & Services → Enable these APIs:
   - Gmail API
   - Google Calendar API
   - Google Drive API
   - Google Tasks API
4. Go to APIs & Services → Credentials → Create OAuth 2.0 Client
5. Application type: Web application
6. Add authorised redirect URIs:
   - `http://localhost:3000/api/auth/callback/google` (for local dev)
   - `https://your-app.vercel.app/api/auth/callback/google` (for production)
7. Copy Client ID and Client Secret

**C. Anthropic API key (Claude AI)**
- Go to https://console.anthropic.com → API Keys → Create key
- Free tier: $5 credit to start

**D. NextAuth secret**
- Run in terminal: `openssl rand -base64 32`
- Or use any long random string

---

### Step 2 — Run locally first (optional but recommended)

```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env.local
# Fill in all values in .env.local

# Push database schema
npx prisma db push

# Start dev server
npm run dev

# Open http://localhost:3000
```

---

### Step 3 — Deploy to Vercel (free)

1. **Push to GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial Bella commit"
   # Create a repo on github.com then:
   git remote add origin https://github.com/YOUR_USERNAME/bella-ai.git
   git push -u origin main
   ```

2. **Connect to Vercel**
   - Go to https://vercel.com → Sign up with GitHub
   - Click "New Project" → Import your bella-ai repo
   - Framework: Next.js (auto-detected)

3. **Add environment variables in Vercel**
   In your Vercel project → Settings → Environment Variables, add:
   ```
   DATABASE_URL          = (your Neon connection string)
   NEXTAUTH_URL          = https://your-app.vercel.app
   NEXTAUTH_SECRET       = (your random secret)
   GOOGLE_CLIENT_ID      = (from Google Cloud Console)
   GOOGLE_CLIENT_SECRET  = (from Google Cloud Console)
   ANTHROPIC_API_KEY     = (from Anthropic Console)
   ```

4. **Deploy**
   Click "Deploy" — Vercel builds and deploys automatically.
   Your URL: `https://bella-ai-xxx.vercel.app`

5. **Run database migration**
   In Vercel dashboard → your project → Functions tab, or run locally:
   ```bash
   npx prisma db push
   ```

---

### Step 4 — Publish your Google app (CRITICAL for worldwide access)

1. Go to Google Cloud Console → APIs & Services → OAuth consent screen
2. Click "Publish app"
3. Add your Vercel URL to authorised redirect URIs in Credentials
4. Without this step, only you can sign in!

---

### Step 5 — Submit to Google Search

1. Go to https://search.google.com/search-console
2. Add your Vercel URL as a property
3. Request indexing
4. Wait 1–4 weeks — then "Bella AI student" will show your site!

---

## Optional: Custom domain

1. Buy `bellaos.app` from Namecheap (~₹900/year)
2. In Vercel → Settings → Domains → Add your domain
3. Follow DNS instructions
4. Update NEXTAUTH_URL to your custom domain

---

## Project structure

```
bella/
├── pages/
│   ├── _app.tsx          # App wrapper + session
│   ├── app.tsx           # Main dashboard (all 5 features)
│   ├── index.tsx         # Landing page (bella.html)
│   └── api/
│       ├── auth/
│       │   ├── [...nextauth].ts  # Auth config
│       │   └── register.ts       # Email signup
│       ├── plan.ts         # Daily study plan
│       ├── chat.ts         # Ask Bella chatbot
│       ├── tasks.ts        # Task CRUD
│       ├── prioritise.ts   # AI task prioritisation
│       ├── habits.ts       # Habit tracking + AI reflection
│       ├── insights.ts     # Learning insights
│       └── google/
│           └── sync.ts     # Sync all Google apps
├── lib/
│   ├── ai.ts             # All 5 Claude AI functions
│   ├── google.ts         # Gmail, Calendar, Drive, Tasks
│   └── prisma.ts         # Database client
├── prisma/
│   └── schema.prisma     # Database models
├── .env.example          # Environment template
└── vercel.json           # Vercel config
```

---

## Support

Built with: Next.js · NextAuth · Prisma · PostgreSQL · Claude AI · Google APIs · Vercel
