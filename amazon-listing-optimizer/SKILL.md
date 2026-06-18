---
name: amazon-listing-optimizer
description: Batch Amazon ASIN listing optimization and full-funnel listing analysis for product detail pages. Use when the user asks to optimize Amazon titles, five bullet points, long descriptions, A+ content analysis, Search Terms/ST, target audience, style, selling points, use scenarios, keyword allocation, keyword relevance review, single ASINs, ASIN batches, parent-child variation ASINs, competitor ASIN comparisons, Sorftime MCP listing research, Alexa/COSMO voice-shopping optimization, machine-readable backend attributes, Q&A intent mapping, or lhx-amazon-title/lhx-amazon-bullets style Karpathy Loop scoring. Produces Excel or HTML outputs with keyword relevance pre-review, user confirmation questionnaire, listing copy, positioning analysis, A+ module suggestions, image copy suggestions, keyword mapping, variation keyword distribution, scores, compliance risks, and strict keyword provenance.
---

# Amazon Listing Optimizer

亚马逊 Listing 批量优化与全维度分析智能体。  
Batch Amazon Listing optimization and full-dimensional ASIN analysis agent playbook.

它用于把 ASIN、关键词库、竞品、评论、Sorftime MCP、Apify、Alexa/COSMO 语音购物优化要求或用户上传资料，转化为可审核、可批量交付的标题、目标受众、五点、长描述、核心亮点、使用场景、Search Terms/ST、后台属性、Q&A 意图映射、关键词分配、父子体关键词矩阵、合规风险与 Excel/HTML 报告。  
It turns ASINs, keyword libraries, competitors, reviews, Sorftime MCP, Apify, Alexa/COSMO voice-shopping requirements, or uploaded data into reviewable and batch-ready titles, bullet points, long descriptions, target audience, core selling points, use scenarios, Search Terms/ST, backend attributes, Q&A intent maps, keyword allocation, variation keyword matrices, compliance risks, and Excel/HTML reports.

Optimize Amazon product titles and listing fields with a looped "score, improve, rescore, keep or revert" workflow. Support single ASIN, batch ASIN, and parent-child variation ASIN workflows. Prefer connected MCP/API data sources, especially Sorftime when configured; ask the user for uploads only when connected data is unavailable or incomplete.

## Tool Compatibility

这是一个通用智能体工作手册，不绑定单一工具。可用于 Codex、Claude Code、Cursor、Trae、QoderWork、CodeBuddy 或任何能读取项目文件的 AI 编程工具。  
This folder is a tool-agnostic agent playbook. Use it in Codex, Claude Code, Cursor, Trae, QoderWork, CodeBuddy, or any coding agent that can read project files.

- 如果工具支持 Skills，把整个 `amazon-listing-optimizer` 文件夹作为一个 skill 安装。  
  If the tool supports Skills, install the whole `amazon-listing-optimizer` folder as one skill.
- 如果工具支持项目规则或 memory 文件，使用 `AGENTS.md` 作为入口，并保持 `SKILL.md` 与 `references/` 在同一文件夹。  
  If the tool supports project rules or memory files, use `AGENTS.md` as the entrypoint and keep this `SKILL.md` plus `references/` in the same folder.
- 如果工具只支持聊天上下文，先粘贴 `AGENTS.md`，再提供 ASIN 或任务输入。  
  If the tool only supports chat context, paste `AGENTS.md` first, then provide the ASIN/task input.
- 所有引用都保持相对路径，不依赖 Codex 专属路径或隐藏状态。  
  Keep all references relative to this folder. Do not depend on Codex-only paths or hidden state.

## Core Workflow

1. Classify the task as `single_asin`, `batch_asins`, or `parent_child_asins`.
2. Gather ASINs, marketplace, category, brand, product facts, keyword source, banned claims, output format, and 1-3 competitor ASINs per product when available.
3. Run the intake gate before drafting. Output a "Listing Readiness Check" that separates confirmed facts, missing facts, risky claims, and questions for the user. Do not draft customer-facing listing copy until the gate passes, unless the user explicitly requests a provisional draft.
4. Pull source data before asking for uploads:
   - If Sorftime MCP is configured, read `references/sorftime-mcp.md` and use Sorftime-style data for product details, keywords, trends, reviews, ranking, and competitor context.
   - If Apify access is available, read `references/apify-data-pipeline.md` and use the Amazon keyword search, product detail, research orchestration, review scraping, and competitor analysis flow.
   - Use `amazon-product-detail` for target and competitor ASIN data when Apify access is available.
   - Use `amazon-keyword-search` for category keyword discovery.
   - Use `amazon-reviews` or review summaries for pain points, objections, and buyer language.
   - Use local Excel/CSV/JSON/HTML files when they already contain ASIN, keyword, competitor, or review data.
   - If source data is still missing, ask the user to upload/paste only the missing fields.
5. Read `references/compliance-checklist.md`, `references/policy-overlay.md`, and `references/amazon-sensitive-words.md` before scoring copy or Search Terms/ST.
6. Treat rules in `references/policy-overlay.md` as higher priority than generic checklist guidance.
7. For Excel/HTML deliverables, batch jobs, or any keyword embedding task, read `references/output-provenance-schema.md` and create a source inventory plus keyword provenance structure before writing copy.
8. For Alexa, COSMO, voice-shopping, AI-shopping assistant, backend attribute, Q&A intent, or machine-readable optimization tasks, read `references/alexa-shopping-optimization.md` and prioritize entity facts, negative constraints, backend attributes, and voice-intent Q&A over keyword density.
9. Confirm the provided keyword map before writing copy. Do not run standalone keyword scoring or classification in this skill. If keyword scoring/classification is missing and required, route the user to a keyword-analysis/search skill or ask for the analyzed keyword file.
10. Run a keyword relevance pre-review against the current product facts. Assign each upstream keyword a preliminary relevance level: `High`, `Medium`, `Low`, or `None`. Export the pre-review as Excel plus a user questionnaire when many terms or material product choices need confirmation. Do not generate final Listing copy until the user confirms or overrides the relevance review, unless they explicitly request a provisional draft.
11. Build `定位分析` before copywriting: define what the product is, who it is for, style, the core selling points, use scenarios, excluded audiences/claims, and evidence.
12. Optimize title first. The title must answer what the product is, who it is for, and what the key highlight is. Then pass the winning title into bullet optimization so bullets fill long-tail keyword and conversion gaps instead of repeating the title.
13. Generate required parallel Listing fields: title under 75 characters unless stricter verified rules apply, target-audience lines, style within 100 characters, five bullet points, long description up to 10,000 characters when needed, Search Terms/ST within 250 characters, main/sub image suggestion words, A+ module suggestion words, core selling points, use scenarios, backend attributes when relevant, keyword usage confirmation, character count table, scoring table, and compliance notes.
14. Export as Excel or HTML when requested. For batch jobs and final Listing outputs, prefer Excel; for narrative research reports, prefer HTML. Use Chinese sheet/section names. Every embedded keyword must be traceable to its source.

## Intake Gate

Always run this gate before generating listing copy. If the user provides partial data, ask concise follow-up questions instead of filling gaps with assumptions.

Do not put unresolved questions into the final Excel. Discuss missing facts, unsupported claims, and choices with the user first. Generate final Excel only after the needed confirmations are resolved, unless the user explicitly asks for a provisional draft.

### Data Sufficiency Check

When the user uploads SellerSprite, Helium 10, Jungle Scout, Amazon Brand Analytics, Ads, Sorftime, Apify, or other research exports, evaluate whether the dataset is sufficient for the requested output before writing copy.

If the uploaded data is useful but incomplete, guide the user to export the next most valuable data instead of only using what is available. Ask for the minimum additional exports needed.

For SellerSprite-style workflows, recommend exports by need:

| Need | Ask user to export |
| --- | --- |
| Seed keyword discovery | Keyword Mining for the main product phrase and 2-3 synonyms |
| Competitor keyword coverage | Reverse ASIN reports for top 3-10 direct competitors |
| Market and title patterns | Search results export for the main keyword, including title, bullets, brand, price, BSR, sales, variants |
| Parent-child variation planning | Variation/child ASIN export or parent-child mapping with color/size/style attributes |
| Review pain points | Review export, review summary, or negative-review samples for target and top competitors |
| Conversion validation | ABA/Search Query Performance, ad search term report, or order/conversion keyword report when available |
| Compliance review | Current title, bullets, description, A+ text, image text, package text, and ST |
| Alexa/COSMO or voice-shopping optimization | Backend/product attribute export, Q&A/FAQ content, A+ comparison tables, negative constraints, and verified product facts |

Rate data sufficiency:

- `Ready`: enough confirmed product facts, keyword data, and competitor context to draft final copy.
- `Needs product facts`: keyword/competitor data exists, but product facts are missing.
- `Needs keyword depth`: product facts exist, but keyword library is too thin.
- `Needs competitor context`: no clear direct competitors or market/title patterns.
- `Needs variation mapping`: parent-child ASINs exist but child attributes or editable fields are missing.
- `Provisional only`: output can be a draft or keyword plan, but not final publishable copy.

### Confirmed Facts

List only facts directly provided by the user, MCP/API, or uploaded files:

- Product type.
- Brand.
- Marketplace and language.
- Material/base metal.
- Finish/color.
- Size and weight.
- Closure/backing.
- Quantity/package contents.
- Use cases/occasions.
- Existing title, bullets, description, and ST if available.
- Target competitors and keyword source.

### Missing Facts To Ask

Ask for missing facts that affect compliance or conversion:

- Exact dimensions and weight.
- Plating/finish details and whether "gold" means gold tone, gold plated, or solid gold.
- Closure type, backing, ear post material, and whether claims like hypoallergenic, nickel-free, lead-free, waterproof, tarnish-resistant, or lightweight are supported.
- Package contents and packaging type.
- Target audience and exclusions, such as adult women only vs girls/kids.
- Brand name and required brand voice.
- Current listing fields if rewriting an existing ASIN.
- Parent-child variation structure and editable fields if variants exist.
- Additional SellerSprite/keyword exports needed for the requested deliverable.

### Risky Claims

Flag but do not use until supported:

- Hypoallergenic, nickel-free, lead-free, waterproof, tarnish-resistant, 14K/18K, real gold, surgical steel, medical grade, lightweight, durable, premium, gift-ready, warranty, guarantee, safe for sensitive ears.

### User Decision

If data is incomplete, ask the user to choose:

- Provide missing facts and then generate final copy.
- Generate a clearly labeled provisional draft using only confirmed facts.
- Produce only a keyword allocation and missing-data checklist.
- Export more data first, then rerun the workflow.

## Data Availability Rule

Use this priority order:

1. Connected MCP/API data, especially Sorftime MCP when configured.
2. Apify-based Amazon product, keyword, research orchestration, review, and competitor-analysis skills.
3. Local files already present in the workspace.
4. User-uploaded Excel/CSV/JSON/HTML/Markdown files.
5. Pasted ASIN lists, product facts, keyword libraries, or competitor snippets.
6. Provisional generation from partial data, only when clearly labeled as provisional.

Do not require uploads when MCP/API can retrieve the data. If required fields remain missing, return a concise missing-data checklist instead of inventing facts.

## ASIN Modes

### Single ASIN

For each standalone ASIN, output:

- Positioning analysis: product type, target audience, excluded audience/claims, core selling points, use scenarios, and evidence.
- Standalone target audience field: publishable `for ...` audience phrases. Default to at least five lines and add more when relevant and supported, such as `for yourself`, `for women`, `for daughter`, `for mom`, `for wife`, `for girlfriend`, `for wedding guest`, `for vacation outfits`, or `for garden party looks`. Excluded audience phrases and unsupported claims stay in positioning/compliance notes, not in target-audience lines.
- Optimized title.
- Five bullet points covering scenario, material, core benefit, compatibility/specs, and trust or care details.
- Target audience phrases.
- Core selling points.
- Use scenarios.
- Long product description.
- A+ content analysis: product modules, module copy direction, keywords/long-tail terms, and non-overlap notes against bullets.
- Main/sub image copy suggestions when useful: main image note, lifestyle image title, detail image title, scenario image title, package/care image title.
- Style/tone recommendation.
- Search Terms/ST candidates within 250 characters.
- Keyword allocation and rejected keywords.
- Scorecard and compliance notes.

### Parent-Child Variation ASINs

For parent-child ASINs, optimize the variation family first, then child ASINs:

1. Identify shared parent-level keywords: product type, brand, primary use case, core material, and broad audience.
2. Identify child-specific keywords: color, size, scent, flavor, pack count, style, compatibility, material, or variant-specific use case.
3. Keep title structure consistent across children while preserving the differentiating variant attribute.
4. Distribute long-tail keywords across child listings when each child has separate editable copy.
5. Avoid stuffing every child title with the same full keyword set.
6. Produce a variation keyword matrix showing which keywords are covered by parent/shared copy, each child title, each child bullet set, and ST/backend terms.

Child title pattern when suitable:

```text
Brand + Product Type + Variant Attribute + Primary Differentiator + Size/Count/Use Case
```

### Batch Independent ASINs

For unrelated ASIN batches:

1. Process each ASIN independently.
2. Share category-level keyword learnings only when ASINs belong to the same category and buyer intent.
3. Track failed or incomplete ASINs separately.
4. Return a batch summary table with status, score, missing data, and compliance risk for each ASIN.

## External Script Routing

If the workspace includes the GitHub skills or scripts named below, prefer their runner for scoring iterations, then synthesize the result with this skill's compliance and handoff format:

- `lhx-amazon-title`: use for ASIN-based title optimization and competitor ASIN comparison.
- `lhx-amazon-bullets`: use after title optimization; pass the winning title through the script's title parameter when supported.
- `amazon-product-manager-skill`: use for broader product manager context, listing diagnosis, or structured product positioning inputs.
- `amazon-analyse` from `amazon-sorftime-research-MCP-skill`: use for full-dimensional ASIN research when Sorftime MCP is configured.
- `amazon-keyword-search`, `amazon-product-detail`, `amazon-research`, `amazon-reviews`, and `competitor-analyzer`: use as the Apify/AI data collection and competitor insight layer when available.

When those scripts are present, inspect their local `README`, `SKILL.md`, `package.json`, or CLI help before running. Typical runner pattern is `npx bun ...`, but do not guess exact arguments. If scripts are absent, execute the manual loop below.

## Title Optimization Loop

Run 4-8 iterations by default. Stop early if the score is at least 90/100 and the last two iterations do not improve materially.

Score each candidate across five 20-point dimensions:

| Dimension | What to Check |
| --- | --- |
| Technical compliance | Character count, punctuation, capitalization, forbidden claims, marketplace and category rules |
| Mobile hook | The first 60-75 characters communicate product type, strongest differentiator, and buyer-relevant value |
| Keyword quality | Primary keyword precision, coverage, natural order, no stuffing |
| Clarity | Buyer understands what the product is within 3 seconds |
| Click intent | Simulated buyer preference versus direct competitors |

For each iteration:

1. Score the current title and identify the 1-3 biggest constraints.
2. Create one improved candidate, changing only what serves the constraints.
3. Rescore the candidate.
4. Keep the candidate only if it improves total score without creating compliance risk or factual overreach.
5. Preserve product facts exactly; never invent certifications, compatibility, dimensions, materials, quantities, or performance claims.

Title construction preference:

```text
Brand + Product Type + Primary Differentiator + Key Attribute/Use Case + Size/Quantity/Compatibility
```

Use this as a flexible pattern, not a rigid formula. Category rules and buyer clarity win over template purity.

## Bullet Optimization Loop

Optimize five bullet points after the winning title exists. Run 4-8 iterations by default.

Score each bullet set across five 20-point dimensions:

| Dimension | What to Check |
| --- | --- |
| Compliance | No unsupported medical, safety, warranty, superlative, or competitor claims |
| Benefit conversion | Each feature passes the "so what" test and states buyer value |
| Keyword strategy | Uses relevant long-tail terms not already overused in the title |
| Pain coverage | Addresses review-mined objections, friction, and use-case concerns |
| Conversion force | Clear, specific, scannable, and confidence-building |

Bullet structure:

1. Lead each bullet with a concise benefit label only when appropriate for category style.
2. Convert feature to benefit: feature -> use case -> buyer outcome.
3. Use one main idea per bullet.
4. Avoid repeating the same keyword phrase across bullets.
5. Keep claims evidence-bound and product-fact-bound.

Required bullet logic:

- Layer 1, core selling point: each bullet should clearly serve a buyer pain point, decision concern, or product difference.
- Layer 2, product information: support the selling point with confirmed features, dimensions, material, audience, or use scenario.
- The bullet set must cover both emotional/decision value and concrete product facts.
- Do not make all five bullets pure keyword carriers or pure feature lists.

For fashion, jewelry, accessories, apparel, or other outfit-driven products, bullets must be more specific than generic benefit copy:

- Cover outfit pairing: casual looks, dresses, blouses, linen outfits, vacation outfits, workwear, date outfits, party looks, or other supported styling contexts.
- Cover use scenes when relevant: daily wear, photo shoots, wedding guest looks, weddings, beach/seaside, date night, going out, work, travel, party, garden party, dinner, or vacation.
- Cover wearer styling when relevant and supportable: short hair, long hair, updos/buns, hair tucked behind the ear, and face-shape flexibility. Avoid absolute claims such as "suits every face shape" unless phrased softly, for example "easy to style with many face shapes".
- Capture mood and aesthetic value when supported: playful, cheerful, sculptural, feminine, polished, vacation-ready, or outfit-brightening.
- Keep every styling claim tied to the visible product design, material, color, stone, shape, or confirmed use case.

Target audience output may use two complementary formats:

- Audience persona notes: short grouped explanations such as self-buyers, women of different ages, gift recipients, fashion lovers, nature-inspired/boho style shoppers, wedding guests, vacation travelers, or photo-focused shoppers.
- Publishable `for ...` phrases: concise phrases for Excel/listing fields, such as `for yourself`, `for women`, `for girlfriend`, `for mom`, `for daughter`, `for wedding guest`, `for photo shoots`, or `for garden party looks`.

Do not target children, young girls, or teens unless the product facts, safety positioning, size, and marketplace/category context support it. If the user provides a generic audience example that includes young girls or teens, adapt it conservatively for the actual product.

Recommended bullet roles:

| Bullet | Role |
| --- | --- |
| 1 | Core selling point: strongest pain point or purchase reason plus key difference |
| 2 | Product information: material, finish, construction, size, quantity, or confirmed specification |
| 3 | Audience and use scenario: who it is for and where/when it is used |
| 4 | Differentiating benefit: comfort, design, care, compatibility, or competitor gap |
| 5 | Decision reassurance: package contents, care note, limitation, or trust-building fact |

## Long Description, A+, And ST Rules

Long description:

- May be up to 10,000 characters when the marketplace/template allows it.
- Use it to supplement the bullets with brand image, product story, scenario language, and buyer reassurance.
- Do not simply repeat the five bullets.
- Keep all claims evidence-bound and avoid unsupported guarantees, certifications, medical/safety claims, or exaggerated performance language.

A+ analysis:

- Output product module suggestions rather than only copy blocks.
- For each module, include module purpose, buyer question answered, suggested visual/content direction, keywords/long-tail terms, and non-overlap note.
- A+ must complement title, bullets, and long description; do not repeat the same five bullet messages word-for-word.
- Use A+ to cover long-tail terms, comparison tables, material/finish explanation, usage scenes, care/limitations, brand tone, and FAQs when supported.

Main/sub image copy:

- Keep image text short and factual.
- Do not use promotional badges, pricing, review/rating language, warranty, guarantees, or unsupported claims.
- Main image should generally avoid added marketing text if marketplace/category image rules require a clean product-only main image.
- Sub-image text can explain confirmed material, finish, closure/backing, use scenarios, package contents, and care.

Search Terms/ST:

- Keep ST within 250 characters unless a stricter marketplace/category rule is provided.
- Do not repeat visible title/bullet/A+/description phrases unnecessarily.
- Do not add plural variants when singular already covers the same search intent.
- Do not include competitor brands, trademarks, ASINs, copyrighted terms, or other infringement risks.
- Do not include advertising words or promotional language such as sale, discount, best, top, cheap, free, guarantee, or similar hype terms.
- Use spaces between terms; avoid commas unless the marketplace template requires them.

Character counts:

- When the user asks for total characters or when exporting Excel/HTML, include `????`.
- Count characters including spaces and punctuation.
- Track at least: title, target audience total, each target-audience line, style, five bullets total, each bullet, long description, ST, A+ analysis/module copy, main/sub image copy, and total. Target audience is a flexible multi-line parallel Listing field, not part of the five-bullet character count.
- Style must be 100 characters or fewer, including spaces and punctuation.
- If a module is not generated, mark it as `not included` instead of silently omitting it.
- Every ST term must appear in `关键词追踪` with source and reason.

## Keyword Handling

When a keyword library or analyzed keyword file is provided, confirm it before copywriting:

1. Confirm the keyword source: user-provided seed, keyword-analysis skill output, SellerSprite/Helium 10/Jungle Scout export, ABA/SQP, Ads, Sorftime, Apify, reviews, or competitor data.
2. Confirm whether the file already contains keyword scoring, categories, tags, personas, or intent groups. Do not recreate those analyses inside this skill.
3. Reject only keywords that are clearly irrelevant, misleading, competitor-trademark, claim-heavy, or unsupported by product facts.
4. Attach provenance to each used keyword: source file/API, source sheet/table, source ASIN when available, source metric, transformation, decision, and risk note.
5. Assign confirmed keywords to title, bullets, backend/search terms, or do not use.
6. Use the confirmed keyword map to evaluate title and bullet candidates.

Strict provenance rule: no keyword may be embedded in title, bullets, description, A+, image text, or Search Terms/ST unless it appears in a source table/API/export, is a user-confirmed product fact, or is explicitly marked as a manual seed. Use `references/output-provenance-schema.md` for the required provenance fields.

Boundary rule: keyword scoring, keyword classification, persona labels such as `Clean`, `factual`, or `masculine`, and broad keyword strategy belong in a keyword-analysis skill. In this skill, only confirm whether those values exist, whether they are relevant to the Listing, and where confirmed keywords are used.

When the final phrase differs from the raw keyword, record the transformation. Example: if the raw keyword is `gold flower earrings` but the product is confirmed only as gold tone, the final phrase may be `gold tone flower earrings`, and the transformation must be recorded.

### Keyword Relevance Pre-Review

Before drafting final Listing copy, compare every upstream keyword against the current product facts and assign a preliminary relevance level:

| Level | Meaning | Allowed Use |
| --- | --- | --- |
| High | Directly matches product type, material/color, form, audience, or confirmed use case. | Title, target audience, bullets, description, A+, image copy, or ST. |
| Medium | Related to style, occasion, outfit context, broad category, or shopper language, but not the exact product noun. | Bullets, description, A+, image copy, or ST; use in title only when natural and evidence-bound. |
| Low | Broad, weak, ambiguous, or requiring visual/product confirmation such as size, silhouette, mood, or special occasion. | Hold for questionnaire or low-priority ST only after user confirmation. |
| None | Contradicts product facts, is unsupported, infringes, or implies a false feature/claim. | Reject and record the reason. Do not embed. |

Use product-fact gates before traffic metrics. High search volume cannot override mismatch. For example, reject or hold terms for clip-on, hoop, drop, dangle, pearl, silver, enamel, hypoallergenic, lightweight, 14K, solid gold, kids/girls, bridesmaid, or named stones unless the user confirms that exact fact.

If the relevance review contains any `Low`, `None`, or high-impact `Medium` terms, output a user questionnaire before final copy. The questionnaire should ask the user to approve, downgrade, reject, or confirm facts for those terms. Do not hide this questionnaire inside the final Listing workbook; use it as a review step before generating the final Listing Excel.

Treat social-community, Reddit, Etsy, fashion-community, or trend-report notes as style and scenario insight sources, not as product-fact sources. They may support styling angles such as nature-inspired looks, statement-piece pairing, metal-tone matching, garden party, summer/vacation outfits, beach scenes, date night, party wear, and hair/outfit styling. They must not create unsupported claims such as handmade, natural material, enamel, resin, real flower, ceramic, sterling silver, Lily of the Valley, or specific flower species unless the product facts confirm them.

When social or trend insights are provided, include them in the relevance pre-review with source type `social_trend_note` or `user_supplied_reddit_note`, then classify each insight as High/Medium/Low/None against the current product. Use confirmed insights in bullets, A+, image copy, long description, and ST before using them in the title.

Recommended questionnaire fields:

| Field | Purpose |
| --- | --- |
| Keyword | Upstream keyword or phrase. |
| Preliminary relevance | High, Medium, Low, or None. |
| Why | Product-fact reason for the preliminary judgment. |
| Source | Source workbook/API/sheet/ASIN. |
| User decision | Approve, downgrade, reject, or needs fact confirmation. |
| Confirmed product fact | The exact fact the user confirms, such as CZ stones, chunky shape, large size, or beach use. |
| Notes | Optional user instruction. |

For Alexa/COSMO-ready outputs, do not chase keyword density at the expense of entity clarity. Treat confirmed product attributes, negative constraints, and Q&A intent coverage as ranking inputs alongside keyword demand.

Separate keywords into:

- Primary keyword: should appear naturally in the title.
- Secondary keywords: may appear in title or bullets if natural.
- Long-tail and use-case keywords: usually belong in bullets.
- Backend/search-term candidates: keep separate if they would make public copy awkward.
- Rejected keywords: irrelevant, misleading, competitor trademarks, unsupported attributes, or claim-heavy terms.

Track keyword coverage in a table:

| Keyword | Priority | Source | Used In | Natural? | Notes |
| --- | --- | --- | --- | --- | --- |

Do not force every keyword into visible copy. A listing that reads like keyword stuffing should lose points even if it covers more terms.

If the keyword library is missing but the user expects keyword-led optimization, pause and ask for it, or generate a provisional keyword library from Amazon search, competitor titles/bullets, and review language when tools/data are available.

## Competitor Comparison

When competitor ASINs are available:

1. Extract each competitor's title, bullets, rating count, price, review themes, and repeated claims.
2. Identify category table-stakes language.
3. Identify whitespace: valuable buyer concerns competitors under-explain.
4. Avoid copying distinctive phrasing from competitors.
5. Use competitor data to improve positioning, not to make unsupported superiority claims.

## Output Format

For a single ASIN, return results in this order:

```markdown
## 定位分析
| Item | Result | Evidence |
| --- | --- | --- |
| 是什么 | ... | ... |
| 目标受众 | ... | ... |
| 风格 | ... | ... |
| 核心亮点 | ... | ... |
| 使用场景 | ... | ... |
| 不适用/不能写 | ... | ... |
| 标题逻辑 | ... | ... |

## 目标受众
Flower stud earrings can appeal to several shopper groups. Write this section as grouped audience notes plus `for ...` phrases when useful.

| Group | Why They May Like It | Evidence |
| --- | --- | --- |
| Self-buyers | ... | ... |
| Women who like statement jewelry | ... | ... |
| Gift recipients | ... | ... |
| Fashion and outfit styling shoppers | ... | ... |
| Nature-inspired or boho-style shoppers | ... | ... |
| Wedding guest / photo-focused shoppers | ... | ... |

| # | Publishable Audience Phrase | Evidence |
| --- | --- | --- |
| 1 | for women | ... |
| 2 | for yourself | ... |
| 3 | for daughter | ... |
| 4 | for mom | ... |
| 5 | for wife | ... |
| 6 | for girlfriend | ... |
| 7 | for wedding guest | ... |
| 8 | for vacation outfits | ... |
| 9 | for garden party looks | ... |
| 10 | for photo shoots | ... |
| 11 | for wedding looks | ... |

Excluded audiences or unsupported claims, such as `for girls`, `for sensitive ears`, or `hypoallergenic audience`, must be recorded in positioning/compliance notes instead of replacing target-audience lines.

## Final Title
[optimized title]

Score: XX/100

## Final Bullet Points
1. ...
2. ...
3. ...
4. ...
5. ...

Score: XX/100

## Long Description
[up to 10,000 characters if needed; includes brand image and scenario expansion]

## A+分析
| Module | Purpose | Keywords/Long-tail Terms | Non-overlap Notes |
| --- | --- | --- | --- |
| ... | ... | ... | ... |

## Search Terms/ST
[<=250 characters, no duplicates, no plural stuffing, no infringement, no advertising words]

## 主副图建议词
| Image | Suggested Words | Notes |
| --- | --- | --- |
| Main image | none or product-only rule | ... |
| Sub image 1 | ... | ... |
| Sub image 2 | ... | ... |
| Sub image 3 | ... | ... |
| Sub image 4 | ... | ... |
| Sub image 5 | ... | ... |

## 评分表
| Area | Score | Notes |
| --- | ---: | --- |
| Title technical compliance | XX/20 | ... |
| Title mobile hook | XX/20 | ... |
| Title keyword quality | XX/20 | ... |
| Title clarity | XX/20 | ... |
| Title click intent | XX/20 | ... |
| Bullet compliance | XX/20 | ... |
| Bullet benefit conversion | XX/20 | ... |
| Bullet keyword strategy | XX/20 | ... |
| Bullet pain coverage | XX/20 | ... |
| Bullet conversion force | XX/20 | ... |

## 关键词追踪
| Keyword | Priority | Used In | Notes |
| --- | --- | --- | --- |

## 关键词分配确认
| Destination | Keywords |
| --- | --- |
| Title | ... |
| Bullets | ... |
| Backend/Search Terms | ... |
| Rejected/Do Not Use | ... |

## What Changed
- ...

## Compliance Notes
- ...

## 已确认前提
- ...
```

If the user wants only copy, provide the final title and five bullets first, then a short note that `评分表` is available.

For Excel output, create these sheets:

- `资料确认`: ASIN, parent ASIN, mode, marketplace, confirmed product facts, source list, readiness status.
- `关键词相关性预审`: ASIN, keyword, preliminary relevance High/Medium/Low/None, reason, source, suggested destination, risk, user decision, confirmed fact.
- `用户确认问卷`: ASIN, question, related keyword, why it matters, options, user answer, impact on Listing.
- `定位分析`: ASIN, product type, target audience, style, excluded audience/claims, core selling points, use scenarios, title logic, evidence.
- `优化文案`: ASIN, title, target audience lines, style <=100 characters, bullet 1-5, long description, ST, core selling points, use scenarios, final copy scores.
- `主副图建议词`: ASIN, image slot, suggested words, visual direction, keyword/long-tail terms, compliance notes.
- `A+分析`: ASIN, module, module purpose, buyer question, suggested content/visual direction, module suggestion words, keywords/long-tail terms, non-overlap notes.
- `关键词追踪`: ASIN, keyword, source, source sheet/table, source ASIN, destination, used field, transformation, naturalness, risk note.
- `评分表`: title score, bullet score, compliance score, conversion score, Alexa/COSMO score when applicable.
- `合规风险`: ASIN, field, issue, risk label, fix.
- `父子体矩阵`: parent-child keyword and copy distribution when variants exist.
- `后台属性`: include when backend attributes or hard-filter fields are part of the task.
- `语音问答`: include when Q&A intent mapping is requested or useful for Alexa/COSMO enrichment.
- `负向约束`: include when explicit exclusions reduce mismatch, hallucination, or return risk.

Do not output `市场品牌分析`, `关键词评分`, `关键词分类`, `关键词合并评分`, or `待确认问题` sheets from this skill. Resolve pending questions in chat before final Excel.

For HTML output, include executive summary, per-ASIN listing sections, `资料确认`, `定位分析`, `优化文案`, `A+分析`, `关键词追踪`, `评分表`, `合规风险`, `父子体矩阵` when needed, and compliance risks.

## Quality Gate

Before finalizing, verify:

- Title and bullets follow the current compliance checklist and any loaded policy overlay.
- Public-facing title, bullets, long description, A+ text, image text, video captions, packaging copy, buyer messages, and Search Terms/ST have been scanned against `references/amazon-sensitive-words.md`; internal report labels such as "review", "rating", or "score" are allowed when they are not customer-facing copy.
- Alexa/COSMO outputs do not claim `Alexa's Choice`, `Alexa Recommended`, `Endorsed by AI`, or any unsupported assistant endorsement.
- The full title stays within the active limit, including the user-provided 75-character rule when no stricter verified rule overrides it.
- The title clearly answers what the product is, who it is for, and the key highlight.
- The first 60-75 title characters work as the mobile search hook.
- `定位分析` is complete: target audience, excluded audience/claims, core selling points, and use scenarios are supported by product facts, keyword data, competitor/review language, or user confirmation.
- Final output includes standalone `目标受众` lines using `for ...` phrases, parallel with title, bullets, long description, and ST. Provide at least five and add more when relevant, supported, and useful.
- No claim exceeds provided evidence.
- Bullets do not duplicate the title's main keyword phrase unnecessarily.
- Bullet set follows the two-layer logic: core selling point/pain point/difference first, then product facts such as feature, size, audience, and scenario.
- Long description can expand to 10,000 characters when needed and adds brand image or scenario depth instead of repeating bullets.
- A+ analysis complements the bullets and uses long-tail terms without repeating bullet copy word-for-word.
- Main/sub image copy is factual, short, and does not use promotional badges or unsupported claims.
- ST is within 250 characters, avoids repeated terms/plural stuffing, contains no infringement risks, and avoids advertising words.
- Style is 100 characters or fewer.
- `字符统计` includes title, target audience total, each target-audience line, style, bullets, long description, ST, A+ analysis/module copy, main/sub image copy, and total when those modules are generated. Target audience is counted separately as a flexible multi-line parallel field.
- Keyword library has been confirmed, sourced, and mapped before final copy is written.
- Final copy includes a `关键词追踪` provenance table when a keyword library was provided.
- Every embedded keyword traces back to a source file/API/sheet/ASIN, user-confirmed product fact, or explicit manual seed.
- No unresolved questions are hidden in Excel; discuss them with the user before final output.
- Batch outputs include per-ASIN status, scores, and missing-data notes.
- Parent-child ASIN outputs include a variation keyword matrix.
- Copy is natural English for the target marketplace.
- All product facts, quantities, compatibility, materials, and dimensions match source data.
- The final answer flags missing information instead of filling gaps with invented details.
