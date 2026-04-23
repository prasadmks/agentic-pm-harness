You are building a structured company context. Follow these steps exactly and in order.

---

## Step 1 — Ask for the website URL

Use the AskUserQuestion tool to ask:

> "What is your company's website URL? (This is required to get started.)"

Do not proceed until you have a URL. If the user says they don't have one or it's not public, ask them to paste a description of the company instead and skip to Step 3 using that as your source material.

---

## Step 2 — Fetch and scrape the website

Use the WebFetch tool to retrieve the URL the user provided. Also try fetching `/about`, `/product`, `/pricing`, and `/team` subpages if they exist, to gather as much context as possible.

From everything retrieved, extract and note internally:
- Company name and tagline
- Mission or vision statement
- Product or service description
- Named features or capabilities
- Target customer signals (job titles, industries, company sizes mentioned)
- Business model signals (pricing page, "plans", "enterprise", "free trial", etc.)
- Any named competitors or comparison language
- Founding story or team background
- Any metrics, customer logos, or social proof

If fetching fails entirely, use AskUserQuestion to ask the user to paste a brief company description, then proceed with that.

---

## Step 3 — Ask clarifying questions (one at a time, options only)

Analyse what you scraped. Identify which of the following areas are unclear, missing, or ambiguous. For each gap you find, ask ONE question at a time using AskUserQuestion. Pre-populate the options based on your best inference from the scraped content — the user selects or picks "Other".

Ask only the questions that are actually needed based on what was unclear. Do NOT ask about things the website made obvious. Aim for 3–6 questions maximum total.

**Example question areas and how to frame them:**

- **Competitors unclear** → List 4–5 companies you inferred as likely competitors from the space, plus "(E) Other — please specify". Ask: "Which of these are actual competitors? Select all that apply."
- **Primary buyer unclear** → List 3–4 job titles inferred from the website copy. Ask: "Who is the primary buyer? Select the closest match."
- **Business model missing** → Offer: (A) Subscription / SaaS, (B) Usage-based, (C) Enterprise contract / custom pricing, (D) Marketplace / transactional, (E) Other. Ask: "Which pricing model fits best?"
- **Core value prop vague** → Offer 3–4 value prop angles inferred from the product description. Ask: "Which is the primary value proposition?"
- **Company stage unclear** → Offer: (A) Pre-revenue / stealth, (B) Early-stage (seed–Series A), (C) Growth-stage (Series B+), (D) Established / profitable, (E) Public company. Ask: "Which best describes the company's stage?"

Rules:
- Always use AskUserQuestion — never ask in plain text
- Always give lettered options (A, B, C...) — never open-ended
- Always include an "Other — please specify" option as the last choice
- Ask questions one at a time; wait for each answer before asking the next
- If the user picks "Other", accept their free-text answer and continue

---

## Step 4 — Generate and save 4 context files

Once you have enough information, generate all 4 files and save them into a `company-context/` folder in the current working directory. Create the folder if it doesn't exist.

### File 1: `company-context/company-overview.md`

Include:
- Company name, tagline, website
- Parent company or division (if applicable)
- Stage / size (headcount, funding, revenue if known)
- Mission statement
- Business model (how they make money)
- Founding story (brief)
- 4–6 key facts as bullet points (metrics, recognitions, scale indicators)

### File 2: `company-context/user-persona.md`

Include:
- Persona name and role title
- Industry, company size, geography
- Goals (3–4 bullet points)
- Pain points (3–4 bullet points)
- How they discover and buy (self-serve vs. sales-led, budget authority)
- A "day in the life" narrative paragraph (3–5 sentences, written in third person, describing a realistic workday moment where this product is relevant)

### File 3: `company-context/product-description.md`

Include:
- What it is (one sentence)
- Core value proposition
- Key features (bulleted list with one-line descriptions each)
- Use cases (3–5 specific scenarios)
- How it works (high-level, 3–5 steps or a brief paragraph)
- Integrations or ecosystem (if known)

### File 4: `company-context/competitive-landscape.md`

Include:
- Overview paragraph positioning the company in its market
- Named competitors with 2–3 sentence descriptions each
- A markdown comparison table with at least 3 competitors and 5+ dimensions (e.g. core strength, target customer, pricing model, key differentiator, weakness vs. this company)
- Positioning summary: where this company wins, where it cedes ground, and what its fastest-growing moat is

---

## Step 5 — Print a summary

After all files are saved, print a brief confirmation in this format:

```
## Company context created

4 files saved to `company-context/`:
- company-overview.md — [one-line description of what's in it]
- user-persona.md — [one-line description]
- product-description.md — [one-line description]
- competitive-landscape.md — [one-line description]

**Key assumptions made:** [list any inferences you made that the user did not explicitly confirm]
```
