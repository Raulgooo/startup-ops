# Startup-Ops

**Claude Code skill for AI-powered market validation and idea exploration**

Compresses weeks of market research into hours. Automates competitor intel, customer pain mining, and validation scoring so you can make better decisions about what to build.

**Primary use case**: Deep validation of your startup idea (positioning, features, go-to-market)  
**Secondary use case**: Explore opportunities in specific markets when you're curious

This is not a viral tool. This is an internal research automation system for high-agency builders who want data-driven conviction.

---

## Core Modes

### 1. `validate` — Deep Validation Report

**What it does**: Comprehensive validation of your idea with A-F scoring across 10 dimensions.

```bash
python startup-ops.py validate --idea "Self-hosted auth infrastructure with flat-rate pricing"
```

**Output**:
- **Market Fundamentals** — TAM/SAM/SOM, growth trends for your vertical
- **Competition Landscape** — Direct/indirect competitors, positioning gaps
- **Customer Reality** — Who pays, why now, willingness to pay signals
- **Technical Feasibility** — Build complexity, time to MVP, stack fit
- **Business Model** — Pricing strategy, unit economics
- **Distribution** — GTM channels, customer acquisition
- **Moats & Defensibility** — Network effects, switching costs, IP
- **Risks** — Security, compliance, support burden, market timing
- **Capital Efficiency** — Can you bootstrap to first revenue?
- **Founder-Market Fit** — Your edge, domain expertise, unfair advantages

**Composite Score**: A-F recommendation (A/B = build, C+ = needs more data, C- = pivot)

**Use case**: "Is this idea worth 6 months of my life?"

---

### 2. `competitor-scan` — Automated Competitor Intel

**What it does**: Scrapes Product Hunt, GitHub, Crunchbase, G2 reviews for auth competitors.

```bash
python startup-ops.py competitor-scan --category "authentication infrastructure"
```

**Output**:
- Direct competitors (Auth0, Clerk, Supabase Auth, Firebase, Cognito)
- Self-hosted alternatives (Keycloak, Authentik, Ory)
- Feature matrix (what they have, what they're missing)
- Pricing comparison (per-MAU vs flat-rate)
- Customer complaints (Reddit, G2, HN threads)
- Funding/runway estimates (how long until acquisition pressure?)
- **Positioning gaps** — underserved segments, unmet needs

**Deliverable**: `reports/competitors-auth-infra.md` with battle cards

**Use case**: "Where is Auth0 weak? What do developers hate about Clerk?"

---

### 3. `market-size` — TAM/SAM/SOM Calculator

**What it does**: Estimates addressable market with multiple methodologies.

```bash
python startup-ops.py market-size --vertical "authentication" --segment "self-hosted B2B"
```

**Methodologies**:
- **Top-down**: Gartner/Forrester reports, auth market sizing
- **Bottom-up**: Addressable devs × adoption rate × ARPU
- **Comparable**: Keycloak adoption, Authentik scale as proxies

**Output**:
```
TAM: $8.2B (auth infrastructure globally, Gartner 2025)
SAM: $420M (self-hosted auth for B2B SaaS, 5% of TAM)
SOM: $2.1M (realistic 3yr capture @ $50 ARPU, 3,500 customers)
Growth: 23% CAGR (AI agent adoption driving demand)
Confidence: B+ (bottoms-up validated against Keycloak data)
```

**Use case**: "Is agent auth a $10M market or $100M market?"

---

### 4. `customer-research` — Pain Point Mining

**What it does**: Scrapes Reddit, Hacker News, Twitter, GitHub issues for developer complaints about auth.

```bash
python startup-ops.py customer-research --problem "authentication pain points"
```

**Sources**:
- Reddit: r/webdev, r/golang, r/selfhosted
- HN: "Ask HN: Auth provider recommendations"
- Twitter/X: Auth0 complaints, Clerk pricing threads
- GitHub: Issues on popular auth libraries

**Output**:
- **Pain clusters** (per-user pricing, vendor lock-in, complex setup, agent auth gap)
- **Jobs-to-be-done** per segment (indie devs, B2B SaaS, enterprise)
- **Willingness to pay** (signals from pricing discussions)
- **Current alternatives** (what they use now, workarounds)

**Deliverable**: `reports/customer-pain-auth.md` with quotes and sources

**Use case**: "What do developers actually complain about in auth providers?"

---

### 5. `positioning` — Differentiation Strategy

**What it does**: Analyzes competitive landscape and defines your unique position.

```bash
python startup-ops.py positioning --idea "SharkAuth" --competitors "Auth0,Clerk,Supabase"
```

**Output**:
- **Positioning map** (2x2: Pricing model vs Control/flexibility)
- **Archetype**: Open Source Challenger, Self-Hosted Alternative, Developer-First
- **One-liner**: "Self-hosted auth that beats Clerk without vendor lock-in"
- **Tagline**: "Auth infrastructure you control. $49/mo flat."
- **Messaging pillars**:
  1. **Control** — Your data, your server, your rules
  2. **Economics** — Flat pricing, not per-MAU extortion
  3. **Speed** — Single binary, 60s deploy
- **Proof points** — What evidence backs your claims?

**Use case**: "How do we position against Clerk without just saying 'self-hosted'?"

---

### 6. `mvp-scope` — Feature Prioritization

**What it does**: Defines minimum viable product based on validation data.

```bash
python startup-ops.py mvp-scope --idea "SharkAuth agent auth"
```

**Output**:
- **Must-have (v0.1)** — Core OAuth 2.1, embedded dashboard, SQLite, single binary
- **Should-have (v0.2)** — Agent auth primitives, tool-level permissions, MCP support
- **Nice-to-have (v0.3+)** — Multi-tenancy RLS, Stripe integration, migration CLI
- **Anti-features** — SAML, LDAP, enterprise SSO (defer until revenue)
- **Success metrics** — 10 self-hosted deployments, 3 paying customers, $500 MRR

**Use case**: "What ships in SharkAuth v0.1 for launch vs what waits?"

---

### 7. `explore` — Idea Discovery (For Fun)

**What it does**: Scans markets for startup opportunities based on trends/verticals.

```bash
python startup-ops.py explore --vertical "AI infrastructure" --trends "agent ecosystems"
```

**Process**:
1. Scrapes HN, Reddit, Twitter for emerging complaints
2. Clusters pain points into themes
3. Checks if solutions exist (quality, gaps)
4. Scores opportunities by market size + urgency
5. Synthesizes concrete startup concepts

**Output**: 5-10 ranked ideas with:
- Problem statement + evidence
- Market size estimate
- Existing solutions analysis
- Technical approach sketch
- Monetization hypothesis

**Example output**:
```
1. Agent Authentication (Score: A-)
   Problem: No self-hosted auth for AI agents with tool-level permissions
   Evidence: 47 HN comments, 12 Reddit threads, 0 solutions
   Market: $50M subset of auth infra
   
2. MCP Server Marketplace (Score: B+)
   Problem: Discovery problem for Model Context Protocol servers
   Evidence: Claude users asking "what MCPs exist?"
   Market: $10M (small but growing fast)
```

**Use case**: "What else is broken in AI infra besides auth?"

---

## Tech Stack

**Runtime**: Python 3.11+  
**LLM**: Claude 3.5 Sonnet (via Anthropic API)  
**Scraping**: Playwright (headless Chrome)  
**Storage**: JSON files in `data/`, Git-versioned  
**Reports**: Markdown → PDF (optional, via Pandoc)

**APIs used**:
- Anthropic API (analysis)
- Crunchbase API (competitor funding)
- Reddit API (pain point mining)
- GitHub API (issue/star scraping)
- Google Custom Search (market research)

**No external dependencies for core validation** — works offline with cached data.

---

## Installation

```bash
# Clone repo
git clone https://github.com/[your-username]/startup-ops.git
cd startup-ops

# Install dependencies
pip install -r requirements.txt
playwright install chromium

# Configure API keys
cp .env.example .env
vim .env  # Add ANTHROPIC_API_KEY, CRUNCHBASE_KEY (optional)

# Set your founder profile
cp config/founder.example.json config/founder.json
vim config/founder.json  # Your background, skills, thesis

# Run first validation
python startup-ops.py validate --idea "your idea here"
```

---

## Configuration

**`config/founder.json`** — Your context (injected into every analysis)

```json
{
  "name": "Raúl",
  "background": "CS student, SysAdmin, multi-project founder",
  "skills": ["Go", "Systems programming", "Linux kernel", "Auth/security"],
  "domain_expertise": ["Authentication", "DevTools", "Infrastructure"],
  "constraints": {
    "time": "~40hrs/week for building",
    "capital": "Bootstrap first, raise later if needed",
    "geography": "LATAM → global market"
  },
  "thesis": [
    "Developer tools and infrastructure",
    "AI agent ecosystems",
    "Self-hosted alternatives to SaaS"
  ],
  "scoring_weights": {
    "market_size": 0.12,
    "competition": 0.10,
    "customer_pain": 0.15,
    "technical_feasibility": 0.15,
    "time_to_revenue": 0.10,
    "capital_efficiency": 0.10,
    "defensibility": 0.08,
    "distribution": 0.10,
    "regulatory_risk": 0.05,
    "founder_market_fit": 0.05
  }
}
```

**Scoring weights** — Customize based on your priorities:
- High `technical_feasibility` weight if you care about speed to build
- High `capital_efficiency` weight if bootstrapping
- High `market_size` weight if raising VC

---

## Usage Examples

### Example 1: Validate Auth Infrastructure Idea

```bash
python startup-ops.py validate \
  --idea "Self-hosted authentication with agent auth primitives" \
  --output reports/auth-infra-validation.md
```

**What you get**:
- Is agent auth a real market or niche curiosity? (TAM/SAM/SOM)
- Who are the competitors? (Auth0, Clerk, self-hosted alternatives)
- What's the customer pain? (vendor lock-in, per-user pricing, lack of control)
- Should this be MVP or later feature? (scoring + risk analysis)
- What's the pitch? (positioning + messaging)

---

### Example 2: Validate Developer Education Platform

```bash
python startup-ops.py validate \
  --idea "Project-based coding courses for LATAM developers" \
  --output reports/dev-education-validation.md
```

**What you get**:
- Market size for Spanish-language dev education
- Competitors (CodeCrafters, Educative, Platzi)
- Customer segments (university students vs bootcamp grads vs working devs)
- Monetization model (subscription vs course bundles)
- Distribution challenge (organic vs paid acquisition)

---

### Example 3: Compare Pricing Models in Your Market

```bash
python startup-ops.py competitor-scan \
  --category "authentication providers" \
  --focus "pricing models"
```

**Output**: 
- Competitor A: $0.023/MAU → $2,300/mo for 100K users
- Competitor B: $0.02/MAU → $2,000/mo for 100K users  
- Competitor C: $0.00325/MAU → $325/mo for 100K users
- Your positioning: $49/mo flat (unlimited MAU) or $249/mo (premium)

**Insight**: Per-unit pricing creates anxiety. Flat-rate could be differentiator.

---

### Example 4: Find Underserved Customer Segments

```bash
python startup-ops.py customer-research \
  --problem "developer pain points in [your vertical]" \
  --segments "indie hackers,B2B SaaS,enterprise"
```

**Output**:
- **Indie hackers**: Price-sensitive, want simplicity, don't need enterprise features
- **B2B SaaS**: Need compliance, self-hosting, reasonable pricing
- **Enterprise**: Already locked into incumbent, high switching cost

**Decision**: Focus on indie hackers + early B2B, skip enterprise initially.

---

### Example 5: Explore Adjacent Markets

```bash
python startup-ops.py explore \
  --vertical "AI infrastructure" \
  --trends "agent ecosystems,MCP"
```

**Example output**:
```
Found 7 opportunities in AI infrastructure:

1. Agent Authentication (A-)
   Problem: No self-hosted solution for agent-to-agent auth
   Market: $50M subset of auth infra
   
2. MCP Registry/Marketplace (B+)
   Problem: Discovery for Model Context Protocol servers
   Market: $10M but growing 200%/yr
   
3. LLM Cost Tracking Dashboard (B)
   Problem: Teams don't know what they're spending on Claude/GPT
   Market: $25M (cost management SaaS)
   
4. Prompt Version Control (C+)
   Problem: Prompts are code but no Git workflow
   Market: $15M (niche but real pain)
```

**Use case**: You validated your main idea, now exploring what else is broken in the space.

---

## Data Structure

```
startup-ops/
├── config/
│   └── founder.json          # Your profile, weights, thesis
├── data/
│   ├── ideas/                # Your idea backlog
│   │   ├── auth-infra.json
│   │   └── dev-education.json
│   ├── experiments/          # Validation experiments
│   │   └── auth-infra-self-hosted/
│   │       ├── market-size.json
│   │       ├── competitors.json
│   │       └── customer-pain.json
│   └── cache/                # Scraped data cache
│       ├── reddit-complaints.json
│       └── hn-threads.json
├── reports/                  # Generated markdown reports
│   ├── idea-validation.md
│   └── competitors-analysis.md
├── startup-ops.py            # Main CLI
└── modes/
    ├── validate.py
    ├── competitor_scan.py
    ├── market_size.py
    ├── customer_research.py
    ├── positioning.py
    ├── mvp_scope.py
    └── explore.py
```

---

## Scoring System

**10 dimensions, weighted by your profile**:

1. **Market Size** (12%) — TAM/SAM/SOM, growth rate
2. **Competition** (10%) — Intensity, differentiation difficulty
3. **Customer Pain** (15%) — Severity, frequency, urgency
4. **Technical Feasibility** (15%) — Can you build this? How fast?
5. **Time to Revenue** (10%) — Days to first dollar?
6. **Capital Efficiency** (10%) — Bootstrap vs raise
7. **Defensibility** (8%) — Moats, network effects, switching costs
8. **Distribution** (10%) — GTM difficulty, CAC
9. **Regulatory Risk** (5%) — Compliance, legal blockers
10. **Founder-Market Fit** (5%) — Your edge, domain expertise

**Letter grades**:
- **A/A-**: Build immediately, high conviction
- **B+/B**: Promising, validate deeper before committing
- **B-/C+**: Significant gaps, needs pivot or more research
- **C and below**: Kill or major rethink required

**Example**: An auth infrastructure idea might score:
- Market Size: B+ (niche but growing fast)
- Competition: A- (gaps in self-hosted solutions)
- Customer Pain: B (real but not urgent yet)
- Technical Feasibility: A (matches your skillset)
- **Composite: A-** → Build it

---

## Philosophy

**This tool is for builders who**:
- Treat validation like an engineering problem (systematic, data-driven)
- Want conviction backed by evidence, not vibes
- Will kill ideas fast when data says no
- Understand that AI compresses research, not decisions

**Not for**:
- People looking for AI to tell them what to build
- Those who need hand-holding or won't talk to customers
- Spray-and-pray idea generators (system kills bad ideas)

**Core principle**: Automate analysis, not conviction. You still decide what to build.

---

## Roadmap

### v0.1 — Core Validation (Current)
- [x] `validate` mode with 10-dimension scoring
- [x] `competitor-scan` with scraping
- [x] `market-size` TAM/SAM/SOM calculator
- [x] `customer-research` pain point mining
- [x] `positioning` differentiation framework
- [x] `mvp-scope` feature prioritization
- [ ] `explore` idea discovery (in progress)

### v0.2 — Enhanced Research
- [ ] Integration hooks for external platforms (course validation, product launches)
- [ ] Interview script generator + analysis
- [ ] Experiment tracker (validation tests, metrics)
- [ ] Historical tracking (decision log, what changed)

### v0.3 — Automation
- [ ] Recurring scans (weekly competitor updates)
- [ ] Auto-alerts (new competitor launched, pricing changed)
- [ ] Dogfooding integration (track your own validation experiments)

---

## Privacy & Data

- **All data stays local**: Ideas, research, experiments in `data/` directory
- **Git-ignored by default**: `data/` and `cache/` excluded from version control
- **No tracking**: System calls public APIs (Crunchbase, Reddit) but never sends your ideas to third parties
- **LLM calls**: Only to Anthropic API (your account, your data controls)

---

## Why This Exists

Every builder faces the same questions:
- Is this market real or just a niche curiosity?
- Who are my actual competitors and what are they missing?
- What features ship in v0.1 vs defer to later?
- What do customers actually complain about in existing solutions?

Spending 2 weeks on manual research for each idea kills momentum. This tool compresses that work into 4 hours.

If it helps you make better decisions about what to build, it's worth it. Everything else is bonus.

---

## License

MIT — fork it, adapt it, build your own thing.

---

## Acknowledgments

Architecture inspired by [santifer/career-ops](https://github.com/santifer/career-ops). The multi-mode design, HITL philosophy, and structured scoring are all adapted from Santiago's proven system.

---

**Built for builders who want data-driven conviction.**
