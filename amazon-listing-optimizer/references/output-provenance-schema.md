# Output And Keyword Provenance Schema

Use this reference when the user asks for Excel/HTML deliverables, batch Listing optimization, keyword embedding, or strict keyword provenance.

## Scope Boundary

This skill produces Listing optimization deliverables. It must not become a market-research or keyword-analysis workbook.

Keep these tasks outside this skill unless another skill/API already provides the result:

- Market brand analysis.
- Keyword scoring.
- Keyword classification/type modeling.
- Keyword mining strategy.
- Broad market opportunity analysis.

In this skill, only confirm which keyword inputs are available, which source each used keyword came from, and where each confirmed keyword is embedded in the final Listing.

## Source Workbook Models

The final deliverable should be shaped like a complete Listing optimization workbook, not only a copywriting sheet.

Use these source models when the user provides similar exports:

- Brand/product market analysis workbook: use only fields needed to confirm product facts, competitor wording patterns, or source provenance. Do not output a separate market-brand-analysis sheet from this skill.
- Related traffic workbook: base ASIN, related ASIN, relation type, related product title, brand, price, sales, BSR, review, LQS, A+ and traffic time.
- Keyword analysis workbook: keyword, translation, tag/type, traffic, traffic share, traffic acquisition rate, organic/ad distribution, display position, organic rank, ad rank, click conversion rate, competition, weekly search volume, Top 3 click share, Top 3 conversion share, Top 3 ASIN.

## Required Excel Sheets

For Excel output, create Chinese sheet names:

1. `资料确认`: confirmed product facts, input source list, readiness status, and fields already confirmed. Do not include unresolved questions here.
2. `关键词相关性预审`: upstream keyword, preliminary relevance High/Medium/Low/None, product-fact reason, source, suggested destination, risk, and user decision field.
3. `用户确认问卷`: questions the user must answer before final copy, related keyword, why it matters, options, user answer, and Listing impact.
4. `定位分析`: product type, target audience, style, excluded audience/claims, core selling points, use scenarios, title logic, and evidence.
5. `优化文案`: title under 75 characters unless stricter verified rules apply, flexible target-audience lines, style within 100 characters, five bullets, long description up to 10,000 characters when needed, Search Terms/ST within 250 characters, core selling points, use scenarios, scenario/material coverage and final copy scores.
6. `主副图建议词`: image slot, suggested words, visual direction, keyword/long-tail terms, and compliance notes.
7. `A+分析`: product modules, module purpose, buyer question, suggested content/visual direction, module suggestion words, keywords/long-tail terms, and non-overlap notes.
8. `关键词追踪`: mandatory source-and-placement table for every keyword used, rejected, or held. This is tracking/provenance only, not keyword scoring or classification.
9. `评分表`: title score, bullet score, conversion score, compliance score, Alexa/COSMO validation score when applicable.
10. `合规风险`: sensitive words, unsupported claims, trademark risks, prohibited content, field-length risks and suggested fixes.
11. `父子体矩阵`: parent/child keyword and copy distribution when parent-child ASINs or variations exist.
12. `后台属性`: backend attribute map when backend attributes or Alexa/COSMO hard-filter fields are part of the task.
13. `语音问答`: voice-intent Q&A only when Alexa/COSMO or Q&A enrichment is requested.
14. `负向约束`: explicit exclusions or limitations that reduce mismatch, hallucination, or return risk.

Do not create these sheets from this skill:

- `市场品牌分析`
- `关键词评分`
- `关键词分类`
- `关键词合并评分`
- `待确认问题`
- English pending-question sheet names from older drafts

Unresolved questions must be handled in chat before generating the final Excel. If the user asks for a provisional workbook despite unresolved facts, label the workbook status as `草稿/待确认` in `资料确认`, but do not put a question backlog into Excel.

For HTML output, include the same Chinese section names as tables. The HTML may be narrative, but it must still include `资料确认`, `定位分析`, `优化文案`, `A+分析`, `关键词追踪`, `评分表`, and `合规风险`.

## Field Logic Rules

Five bullet points:

- Use two-layer logic: core selling point first, product information second.
- Core selling point means pain point, decision concern, product difference, or buyer outcome.
- Product information means confirmed feature, size, material, audience, use scenario, package count, care, compatibility, or limitation.
- Do not make bullets a duplicate title keyword list.
- For fashion, jewelry, accessories, apparel, and outfit-driven products, include supported styling depth: outfit pairing, daily/photo shoot/wedding/beach/date/going-out/party use scenes, hair styling such as short hair, long hair, and updos, flexible face-shape wording, and the intended mood or atmosphere. Avoid absolute fit claims.

Long description:

- May be up to 10,000 characters when the marketplace/template allows it.
- Use it to add brand image, product story, scenario expansion, and reassurance not fully covered by the bullets.
- Do not simply repeat the five bullets.

A+ analysis:

- Provide product module descriptions, not just copy.
- Include keywords and long-tail terms by module.
- State how each module avoids repeating bullet copy.

Search Terms/ST:

- Keep within 250 characters unless a stricter marketplace/category rule is provided.
- Do not repeat visible copy terms unnecessarily.
- Avoid plural duplicates when singular covers the same intent.
- Do not include competitor brands, trademarks, ASINs, copyrighted terms, or infringement risks.
- Do not use advertising words or promotional language.
- Every ST term must be traceable in `关键词追踪`.

Character count:

- Count characters including spaces and punctuation.
- Include title, target audience total, each target-audience line, style, bullets total, each bullet, long description, ST, A+ module copy/keywords, main/sub image copy, and total.
- Style must be 100 characters or fewer, including spaces and punctuation.
- Target audience is counted separately as a flexible multi-line parallel Listing field, not inside the five-bullet character count.
- If a module is not generated, mark it as `not included`.

## Positioning Analysis Rules

Before writing title, bullets, or description, create `定位分析`.

Required fields:

| Field | Meaning |
| --- | --- |
| 是什么 | Canonical product type in buyer language. |
| 目标受众 | Grouped audience persona notes plus publishable `for ...` audience phrases. Provide at least five `for ...` lines by default and add more when relevant and supported, such as `for yourself`, `for women`, `for daughter`, `for mom`, `for wife`, `for girlfriend`, `for wedding guest`, `for photo shoots`, `for wedding looks`, `for vacation outfits`, or `for garden party looks`, supported by product facts, keyword/source data, or user confirmation. |
| 排除人群/不能写 | Audiences or claims that cannot be supported, such as kids, sensitive ears, hypoallergenic buyers, or solid-gold buyers when unverified. |
| 核心亮点 | Confirmed differentiators such as material, finish, closure, comfort backing, package count, design, or care. |
| 使用场景 | Daily wear, photo shoots, wedding looks, wedding guest outfits, party, office, beach, date night, garden party, gifting, travel, storage, or other scenarios only when supported. |
| 标题逻辑 | How the final title answers what it is, who it is for, and what the key highlight is. |
| 证据 | Product fact, keyword source, review/competitor language, or user confirmation. |

Title rule:

`[Product Type] + [Audience/Use Case] + [Key Highlight or Material/Finish]`

The order can be adjusted for natural English, but the final title must answer all three questions: what is it, who it is for, and why it is worth clicking.

## Keyword Provenance Rules

No keyword may be embedded into final visible copy or Search Terms/ST unless it has a source.

Allowed source types:

- User-confirmed product fact, such as material, package count, finish, closure type, color, size or audience.
- Keyword Mining, Reverse ASIN, Search Results, Brand Analytics, Search Query Performance, ad search term report, Sorftime MCP, Apify export, review/Q&A language, related traffic export or manually pasted competitor data.
- Social-community or trend insight notes, such as Reddit, Etsy, fashion-community, or trend-report summaries, only for style, scenario, outfit, mood, and positioning guidance.
- User-declared seed keyword, clearly marked as `manual_seed`.

For every embedded keyword or phrase, record:

| Field | Required meaning |
| --- | --- |
| Keyword | Normalized keyword or phrase already provided by a keyword-analysis source or user-confirmed input. |
| Exact phrase used | Exact words placed in title, bullet, description or ST. |
| Destination | Title, Bullet 1-5, Description, ST, A+, Image text, Rejected or Hold. |
| Source type | Keyword Mining, Reverse ASIN, Search Results, Related Traffic, ABA/SQP, Ads, Review, Product Fact, Sorftime, Apify, social_trend_note, user_supplied_reddit_note, manual_seed. |
| Source file/API | Workbook name, API/MCP name, pasted text label or manual source. |
| Source sheet/table | Sheet name, endpoint name or table name. |
| Source ASIN | Target ASIN, competitor ASIN, related ASIN or blank for market-level seed. |
| Source metric | Existing metric from the source, if provided. Do not calculate fresh keyword scores in this skill. |
| Transformation | Exact, plural/singular, reordered, partial phrase, translated, product-fact wording or rejected. |
| Reason | Why this keyword is confirmed for that Listing field. |
| Risk | Trademark, unsupported attribute, claim risk, stuffing risk, irrelevant, duplicate or none. |
| Confidence | High, Medium or Low. |

If the exact final phrase differs from the raw keyword, document the transformation. Example: raw `gold flower earrings` becomes `gold tone flower earrings` because the product color is confirmed as gold tone but not solid gold.

## Embedding Rules

- Title may use only the highest-relevance primary keyword plus confirmed product facts.
- Bullets should use secondary, benefit, material, use-case, audience and occasion keywords without repeating the full title phrase unnecessarily.
- Search Terms/ST should prioritize relevant terms not already covered naturally in visible copy, subject to marketplace rules and byte limits.
- Do not create new keyword scores, keyword classes, or personas such as `Clean`, `factual`, or `masculine` unless those values are supplied by the user or a keyword-analysis skill. In this skill, treat them only as style/tone confirmations.
- Do not use competitor brand names, trademarked terms, unsupported certifications, medical claims, allergy claims, fake material claims or unverified audience restrictions.
- Product facts and keywords are different. Product facts such as `zinc alloy`, `stud`, `brushed` or `1 pair` can be used only when confirmed by the user or source data; they still need provenance in `关键词追踪`.
- Rejected keywords are not failures. Record them with reason so the user can see why they were not embedded.

## Parent-Child Rules

For variation families:

- Mark keywords as `parent_shared`, `child_specific`, `backend_shared`, `backend_child_specific`, `rejected` or `hold`.
- Do not force all child ASINs to carry the same full keyword set.
- Child-specific keywords must match the child attribute, such as color, size, style, pack count or material.
- The `父子体矩阵` must show which child covers each keyword and where it is covered.

## Quality Gate

Before final delivery, verify:

- Every embedded keyword has a row in `关键词追踪`.
- Every row in `关键词追踪` points to a source file/API/sheet or confirmed product fact.
- Every final Listing field can be traced back to either product facts, keyword sources, competitor/review language or compliance rules.
- Unsupported phrases are removed and discussed with the user before final output.
