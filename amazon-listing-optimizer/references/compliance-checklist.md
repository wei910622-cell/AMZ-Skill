# Amazon Listing Compliance Checklist

Use this checklist before scoring and again before final output. Treat it as a baseline guardrail; category-specific Amazon rules or user-provided policy updates override it.

Also read `amazon-sensitive-words.md` for the expanded sensitive/forbidden word library. Scan every generated title, bullet, long description, A+ text, image text, video caption, packaging phrase, buyer message, and Search Terms/ST against that file.

## Title Checks

- Keep the title within the current category and marketplace character limit.
- Apply the user-provided 75-character title limit from `policy-overlay.md` unless a verified marketplace/category rule overrides it.
- Prioritize buyer-readable order over keyword stuffing.
- Use the brand name only when it is the correct brand for the product.
- Avoid promotional phrases such as "best", "top rated", "free shipping", "sale", "discount", or price references.
- Avoid time-sensitive claims such as "new", "latest", or "2026" unless factually required and policy-safe.
- Avoid unsupported superlatives such as "number one", "most powerful", "ultimate", or "guaranteed".
- Avoid excessive capitalization, repeated punctuation, decorative symbols, emojis, and non-standard separators.
- Avoid competitor trademarks unless the product is genuinely compatible and the category permits compatibility language.
- Avoid medical, health, safety, environmental, certification, warranty, or performance claims unless the user provides evidence and the category allows the claim.
- Avoid repeating the same word or phrase unnaturally.

## Bullet Checks

- Keep bullets factual, scannable, and tied to buyer benefits.
- Do not include pricing, promotions, coupons, shipping speed, seller contact details, review requests, or external URLs.
- Do not promise outcomes the product cannot guarantee.
- Do not imply official endorsement, certification, or compatibility without proof.
- Do not attack competitors or claim superiority without allowed substantiation.
- Convert features into benefits, but keep the benefit evidence-bound.
- Use long-tail keywords naturally and avoid repeating the title's exact keyword block.
- Make each bullet carry a distinct role.

## Evidence Checks

Require source evidence for:

- Certifications, standards, approvals, and lab-tested claims.
- Compatibility with named models, brands, platforms, or devices.
- Materials, dimensions, weight, capacity, count, warranty, package contents, and country of origin.
- Safety, health, child, pet, food-contact, cosmetic, supplement, or medical-device claims.
- Waterproof, fireproof, shatterproof, leakproof, heavy-duty, non-toxic, organic, eco-friendly, or biodegradable language.

When evidence is missing, downgrade the score and rewrite with neutral wording.

## Final Risk Labels

Use these labels in final notes when needed:

- `OK`: no obvious issue after available checks.
- `Needs evidence`: claim may be usable if the user provides proof.
- `Rewrite recommended`: likely compliance or clarity risk.
- `Do not use`: high-risk, unsupported, or misleading claim.
