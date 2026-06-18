# Policy Overlay

Use this file to store user-provided Amazon rule updates, marketplace-specific constraints, category style guides, or internal brand guardrails.

## How To Use

1. Read this file whenever the user says there are new Amazon rules, category rules, or listing policy updates.
2. Treat rules in this file as higher priority than the baseline compliance checklist.
3. If a rule conflicts with source product facts or with another rule, flag the conflict and choose the stricter compliance-safe interpretation.
4. If this file has no concrete rules yet, tell the user the policy overlay is ready and ask them to paste the new rule text before applying it.

## Current User-Provided Rules

User-provided rule: optimize Amazon titles to under 75 characters unless a marketplace or category-specific verified rule is stricter.

User-provided rule: Listing optimization output must include title, target audience, style, five bullet points, long description, Search Terms/ST, main/sub image suggestion words, and A+ module suggestion words.

User-provided rule: Style field must be 100 characters or fewer, including spaces and punctuation.

## Rule Capture Template

When the user provides rule text, summarize it here in this structure:

```markdown
### Rule Set: [date/source/category/marketplace]

- Scope:
- Effective date:
- Title limits:
- Bullet limits:
- Forbidden words or claims:
- Required formatting:
- Marketplace/category exceptions:
- Source notes:
```

## Application Notes

- Apply category and marketplace limits before generic marketplace advice.
- Prefer strict compliance when rules are ambiguous.
- Preserve the original wording of critical numeric limits, forbidden terms, and effective dates in notes.
- Do not cite this file as an external source; cite it as "user-provided policy overlay" when presenting assumptions.
