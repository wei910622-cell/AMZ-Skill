# Alexa For Shopping / COSMO Optimization

Use this reference when the user asks to optimize a Listing for Alexa for Shopping, COSMO-style knowledge graphs, voice-shopping intent, machine-readable product entities, backend attributes, Q&A intent mapping, or AI shopping assistant recommendations.

This reference complements normal Amazon Listing optimization. It does not replace compliance, keyword provenance, or product-fact verification.

## Core Principle

Prioritize negative constraints, entity relationships, complete factual attributes, and voice-intent matching over keyword density. The goal is to make the product a reliable, low-risk recommendation node.

## Title Structure

Use the title as the primary entity definition.

- Max 80 characters unless the active marketplace rule is stricter.
- Must be grammatically valid and natural.
- Recommended schema: `[Brand] + [Canonical Category] + [Key Differentiator] + [Primary Audience/Use Case]`.
- Avoid subjective fluff such as `Best`, `Top`, `High Quality`, `Premium` unless the claim is verifiable and compliant.
- If the user provides a stricter title limit, such as 75 characters, follow the stricter limit.

## Bullet Structure

Use bullets as atomic fact units.

- Five bullets.
- One primary fact per bullet.
- Each bullet should be a complete sentence.
- Recommended maximum: 200 characters per bullet when the user wants Alexa/COSMO-ready copy.
- Recommended structure: `[Attribute] -> [Quantified Value] -> [Boundary Condition]`.

Prefer bullets that cover:

1. Support/specs: dimensions, material, density, weight capacity, quantity, confirmed construction.
2. Sizing or fit logic: compatibility, dimensions, fit range, age/weight limits when confirmed.
3. Maintenance: cleaning, storage, drying, care limitations when confirmed.
4. Safety/stability: base material, certifications, warnings, age limits, only when verified.
5. Negative constraints: explicit exclusions that reduce mismatch and returns.

## Backend Attributes

Treat backend/product attributes as hard filters before semantic matching. If attributes are missing, ask the user for them instead of inventing them.

Common fields:

- Material
- Dimensions
- Weight or capacity
- Closure type
- Finish or color
- Package count
- Intended use case
- Audience
- Compatibility or fit
- Care instructions
- Safety/certification fields when verified

For Alexa/COSMO optimization, report attribute completion rate and list missing attributes.

## Q&A Intent Mapping

Create 10-15 Q&As when the user requests voice-shopping, Alexa, AI-shopping, or knowledge-graph enrichment.

Format:

`User Intent -> Product Attribute -> Verified Answer`

Rules:

- Questions should sound like natural voice queries.
- Answers must cite specific verified parameters.
- Do not answer with unsupported claims.
- If the answer depends on missing data, mark it as `Needs confirmation` instead of inventing.

Example:

| User Intent | Product Attribute | Verified Answer |
| --- | --- | --- |
| Will these earrings pinch my ears? | Closure and backing | These are stud earrings with soft silicone comfort backing. Hypoallergenic performance is not confirmed. |

## Negative Constraints

Negative constraints reduce hallucination, returns, and mismatched recommendations. Include them directly in bullets, A+ content, Q&A, or assumptions when relevant.

Examples:

- `Constraint: Not hypoallergenic unless verified`
- `Constraint: Not solid gold unless verified`
- `Constraint: Indoor use only`
- `Constraint: Not waterproof`
- `Constraint: Air dry only`
- `Constraint: Not for children under a confirmed age`

Only state negative constraints that are relevant and supportable. Do not create alarming warnings without reason.

## A+ / Knowledge Graph Enrichment

Use A+ content to add scenario-based natural language and comparison tables that are easy to parse.

Recommended sections:

- Scenario narrative: who uses it, when, and why.
- Attribute table: material, dimensions, package count, care, compatibility.
- Comparison table: variant differences or competitor-safe comparison by attributes, not brand attacks.
- Negative constraints and care notes.
- FAQ/Q&A corpus when voice intent is important.

## External Intelligence And Ads

If the user provides external traffic or ad data, use it as a separate source layer. Do not invent it.

Possible source fields:

- Traffic rank or relative market scale.
- Top paid keywords.
- Upstream/referral sources.
- Downstream purchase-intent signals.
- Paid keyword ratio.
- Engagement proxies such as bounce rate, pages per visit, or direct traffic trend.

Use paid external keywords as candidates for Amazon PPC manual exact/phrase campaigns only after relevance and compliance checks. Record provenance in `关键词追踪`.

## Compliance Rules

- Do not claim `Alexa's Choice`, `Alexa Recommended`, `Endorsed by AI`, or similar endorsement language.
- All factual claims must be verifiable from product facts or trusted sources.
- No pricing information in purchase-prompt style copy unless the marketplace/template explicitly allows it.
- If using Amazon Skill / ASK monetization language, use placeholders such as `{PREMIUM_CONTENT_TITLE}` where required and limit example phrases to three.

## Validation Checklist

For Alexa/COSMO-ready output, include a validation table:

| Area | Check |
| --- | --- |
| Title | Grammatically valid and within active character limit. |
| Bullets | Exactly five, complete sentences, one main fact each. |
| Negative constraints | Relevant exclusions included or explicitly marked not applicable. |
| Backend attributes | Attribute completion rate reported. |
| Q&A corpus | 10-15 voice-style Q&As when requested. |
| Compliance | No Alexa endorsement claims and no unverifiable facts. |

## Output Additions

When Excel output is requested and Alexa/COSMO optimization applies, add:

- `Alexa COSMO Validation`: title, bullets, backend fill rate, Q&A count, negative constraints, compliance flags.
- `Backend Attribute Map`: attribute, value, source, confidence, missing/confirmed status.
- `Voice Intent Q&A`: user intent, question, answer, supporting product fact, source, status.
- `Negative Constraints`: constraint, reason, source, affected field, publish status.

When HTML output is requested, include the same sections as tables.
