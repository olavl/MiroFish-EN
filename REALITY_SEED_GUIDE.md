# MiroFish Reality Seed Guide

How to craft effective reality seeds for high-quality multi-agent simulations.

## What Is a Reality Seed?

A reality seed is the input you provide to MiroFish — uploaded documents (PDF, Markdown, or TXT) plus a simulation requirement — that defines the "parallel world" the system will build and simulate. The quality of your seed directly determines the quality of your simulation.

MiroFish processes your seed through a pipeline: **Text → Ontology → Knowledge Graph → Agent Profiles → Simulation Config → Simulation → Report**. Understanding this pipeline is key to writing effective seeds.

---

## The Two Inputs

### 1. Simulation Requirement (text field)

This is the single most important input. It tells the system **what future scenario to simulate**. Be specific and scenario-driven.

**Good examples:**
- "Simulate the public reaction if Company X announces a 40% workforce reduction, focusing on employee social media behavior, union response, and media coverage over 72 hours"
- "Predict how a proposed university tuition increase of 25% would play out on social media among students, faculty, administration, alumni, and local media"
- "Simulate the market and social media response to a major bank announcing it will no longer process cryptocurrency transactions"

**Bad examples:**
- "Simulate public opinion" *(too vague — about what? who?)*
- "What happens with AI?" *(no scenario, no actors, no stakes)*
- "Run a simulation" *(tells the system nothing)*

**Rules for simulation requirements:**
- State the **specific event or decision** being simulated
- Name the **key actors or groups** involved
- Define the **timeframe** if relevant (breaking news = 24h, policy debate = 7 days)
- Indicate what **outcome or dynamics** you want to observe

### 2. Uploaded Documents (PDF, Markdown, or TXT)

These provide the factual foundation — the "reality" part of the seed. The system extracts entities, relationships, and context from these documents to build a knowledge graph.

**File format notes:**
- **PDF**: Text is extracted per page via PyMuPDF. Scanned/image PDFs won't work.
- **Markdown**: Best format for structured input. Headers, lists, and sections are preserved.
- **TXT**: Plain text works fine. Use clear paragraph breaks.
- Maximum upload size: 50MB total
- Multiple files are concatenated and processed together

---

## How to Structure Your Seed Documents

### The Ontology Extraction Stage

MiroFish's first step is extracting an **ontology** — exactly 10 entity types and 6-10 relationship types from your text. Understanding what it looks for helps you write better seeds.

#### Entity Types: What Works

The system needs **concrete, real-world actors that could plausibly post on social media**. It will extract 8 specific types from your content, plus 2 mandatory fallback types (`Person` and `Organization`).

**Write about concrete actors:**
- Individuals with clear roles: "Dr. Sarah Chen, Chief Risk Officer at National Bank"
- Organizations with defined functions: "The Financial Conduct Authority, the UK's financial regulatory body"
- Groups with identifiable membership: "The National Nurses Union, representing 185,000 registered nurses"
- Media outlets with known positions: "The Financial Times, which has historically supported deregulation"

**Do NOT write about abstract concepts as if they were actors:**
- ❌ "Public opinion" — not an entity, can't post on social media
- ❌ "Market sentiment" — an abstraction, not a person or organization
- ❌ "The opposition" — too vague, name specific groups
- ❌ "Supporters and critics" — who specifically?

#### Relationship Types: What Works

The system extracts relationships like `WORKS_FOR`, `REGULATES`, `COMPETES_WITH`, `REPORTS_ON`, `OPPOSES`, `SUPPORTS`. Make these explicit in your text.

**Write explicit relationships:**
- "Professor Martinez teaches in the Computer Science department at Stanford"
- "The FDA regulates drug pricing for pharmaceutical companies"
- "Reuters has been covering the company since the initial IPO filing"
- "The union has filed three grievances against management this quarter"

**Don't assume relationships are obvious:**
- ❌ "Various stakeholders are involved" — who relates to whom, and how?
- ❌ "There is tension" — between whom? what kind?

### The Knowledge Graph Stage

After ontology extraction, MiroFish chunks your text into ~500-character segments and builds a knowledge graph via Zep. This is where factual density matters most.

**Write fact-dense text:**
- Concrete events with dates: "On March 3, the CEO announced restructuring plans at the all-hands meeting"
- Quantified impacts: "The proposed merger would affect 12,000 employees across 8 offices"
- Direct quotes or positions: "The union president stated: 'We will not accept any layoffs without severance guarantees'"
- Historical context: "This is the third round of layoffs in 18 months"

**Avoid padding:**
- ❌ Long philosophical introductions
- ❌ Generic industry background that doesn't mention specific entities
- ❌ Repetitive restating of the same facts
- ❌ Commentary without new information

### Minimum Content Requirements

| Seed Quality | Text Length | Entity Types | Relationships Described |
|---|---|---|---|
| Minimum viable | 2,000 chars | 5-6 distinct actors | 3-4 explicit connections |
| Good | 5,000-10,000 chars | 8-12 distinct actors | 8-10 explicit connections |
| Excellent | 10,000-30,000 chars | 15-20+ actors | 15+ connections with context |

---

## What Makes Agents Come Alive

The system generates agent personas from your knowledge graph. Each agent gets a 2,000-character personality profile including background, personality (MBTI), social media behavior, and stance on the simulated topic.

**To get realistic agents, your seed should include:**

1. **Personality indicators** — How do these people communicate? Are they formal or casual? Aggressive or measured?
   - "The CEO is known for blunt, no-nonsense communication and rarely engages with critics directly"
   - "Student activist Maya Rodriguez runs a popular TikTok account and frequently goes viral with satirical takes"

2. **Motivations** — Why does each actor care about this issue?
   - "The union has been negotiating for 6 months and sees this as a make-or-break moment"
   - "The local newspaper depends on the company for 30% of its advertising revenue"

3. **Background context** — Education, experience, affiliations
   - "Dr. Park completed her PhD at MIT and spent 10 years at Goldman Sachs before joining the regulator"
   - "The company was founded in 2015 by two former Google engineers"

4. **Group dynamics** — Which communities or circles do they belong to?
   - "The engineering team has been organizing informally through a private Slack channel"
   - "Industry analysts from Morgan Stanley, JP Morgan, and Deutsche Bank have all weighed in"

---

## Simulation Configuration: What You Control

The system auto-generates simulation parameters, but understanding the defaults helps you calibrate expectations:

- **Duration**: 24-168 simulated hours (breaking news → short, policy debate → long)
- **Agent behavior**: Activity levels, posting frequency, response delays
- **Platform dynamics**: Twitter-like (fast, viral) or Reddit-like (threaded, community-driven)
- **Time patterns**: Activity follows a daily cycle (low overnight, peak in evening)

The simulation requirement drives these choices. Mentioning urgency ("breaking news", "emergency announcement") produces shorter, more intense simulations. Mentioning ongoing processes ("policy review", "quarterly earnings") produces longer, slower-burn simulations.

---

## Template: High-Quality Reality Seed

Here's a template structure for your seed documents:

```markdown
# [Scenario Title]

## Background
[2-3 paragraphs describing the situation, key events, and timeline]

## Key Actors

### [Actor/Organization 1]
- Role: [specific role in the scenario]
- Background: [relevant history, credentials, affiliations]
- Position: [their known stance or likely reaction]
- Communication style: [how they typically communicate publicly]
- Key relationships: [connections to other actors listed here]

### [Actor/Organization 2]
[same structure]

### [Actor/Organization 3-N]
[same structure — aim for 8-15 actors]

## Key Relationships
- [Actor A] [relationship] [Actor B]: [context for this relationship]
- [Actor C] opposes [Actor D] on [specific issue]: [why]
- [Actor E] reports on [Actor F]: [history of coverage]

## Recent Events
- [Date]: [Event description with actors involved]
- [Date]: [Event description]
[3-5 concrete events that set the stage]

## Simulation Focus
[What specific dynamics or outcomes are you trying to observe?]
```

---

## Common Mistakes and How to Avoid Them

| Mistake | Why It Fails | Fix |
|---|---|---|
| Only abstract entities | System needs social-media-capable actors | Name real people, companies, agencies |
| No relationships stated | Graph has isolated nodes, no interaction paths | Explicitly state who relates to whom |
| Single paragraph of text | Too little data for 10 entity types | Provide 5,000+ characters minimum |
| All actors are the same type | Simulation becomes echo chamber | Mix individuals, institutions, media, regulators |
| Vague simulation requirement | System can't determine what to simulate | State specific event + actors + timeframe |
| Overly long documents (100+ pages) | Text gets truncated to 50,000 chars | Curate the most relevant content |
| No personality/motivation info | Agents become generic and interchangeable | Include how actors think and communicate |
| Missing temporal context | Simulation has no urgency or pacing cues | Include dates, deadlines, urgency signals |

---

## Example: Complete Reality Seed

**Simulation Requirement:**
"Simulate the 48-hour social media response to TechCorp announcing the closure of its Austin, TX office, affecting 2,000 employees. Focus on employee reactions, local government response, competitor recruitment efforts, and media coverage."

**Seed Document (excerpt):**

```markdown
# TechCorp Austin Office Closure

## Background
TechCorp (NASDAQ: TECH) is a mid-cap enterprise software company headquartered in
San Francisco. On March 15, 2026, CEO James Wheeler announced the closure of the
Austin, TX engineering office as part of a "strategic realignment." The Austin office
employs 2,000 people, primarily software engineers and product managers. The office
was opened in 2019 during the company's rapid expansion phase.

## Key Actors

### James Wheeler, CEO of TechCorp
- Background: Former McKinsey partner, joined TechCorp in 2023
- Communication style: Corporate, measured, uses LinkedIn for major announcements
- Known for: Cost-cutting focus since taking over, closed the London office in 2025
- Relationship to employees: Seen as distant, rarely visits satellite offices

### Maria Santos, VP of Engineering (Austin)
- Background: 15-year veteran, built the Austin team from 50 to 2,000
- Communication style: Direct, empathetic, active on Twitter/X
- Position: Reportedly opposed the closure in board discussions
- Relationship: Mentor to many senior engineers, close to local tech community

### Austin Tech Workers Alliance (ATWA)
- Type: Informal employee advocacy group, 800 members on Discord
- Founded: 2025 after the London office closure as a precautionary measure
- Leadership: Anonymous, communicates through a shared Twitter account @ATWAvoice
- Position: Will likely organize protests and public pressure campaigns

### Datastream Inc. (competitor)
- Headquarters: Austin, TX — direct competitor in enterprise software
- CEO Lisa Park has publicly stated interest in hiring displaced TechCorp talent
- Currently has 150 open engineering positions
- Relationship to TechCorp: Lost two major contracts to TechCorp in 2025

### Austin American-Statesman (local newspaper)
- Has covered TechCorp's Austin presence since 2019
- Reporter Jake Morrison has sources inside TechCorp
- Editorial position: Pro-business but protective of local employment

### Mayor Kirk Watson (City of Austin)
- Approved $12M in tax incentives for TechCorp's Austin office in 2019
- Facing re-election in November 2026
- Relationship: Will face public pressure about the tax incentive deal
```

---

## Model Recommendations

MiroFish makes many LLM calls per simulation. Choose your model tier based on what matters:

| Stage | Volume of Calls | Recommended Model | Why |
|---|---|---|---|
| Ontology generation | 1-2 calls | Sonnet or better | Needs structured reasoning |
| Graph building | Via Zep API | N/A (Zep handles this) | — |
| Profile generation | 1 per agent (10-50+) | Sonnet | Balance of quality and cost |
| Simulation config | 3-5 calls | Sonnet | Needs to understand dynamics |
| Simulation runtime | Hundreds per round | Haiku | Cost control for agent chatter |
| Report generation | 5-10 calls | Opus | Final output quality matters most |
| Report chat | Per message | Opus | Interactive analysis benefits from depth |

This repo is configured with Sonnet 4 for general use and Opus 4.6 for report generation/chat via Bedrock + LiteLLM proxy.
