# Amazon Listing Optimizer Agent Entry

亚马逊 Listing 批量优化与全维度分析智能体入口。

Universal agent entrypoint for batch Amazon Listing optimization and full-dimensional ASIN analysis.

本文件适用于 Codex、Claude Code、Cursor、Trae、QoderWork、CodeBuddy 等能读取项目文件的 AI 编程工具。  
Use this file as the universal entrypoint for Codex, Claude Code, Cursor, Trae, QoderWork, CodeBuddy, or any agent that can read project files.

目标：让智能体基于 ASIN、关键词库、竞品、评论、Sorftime MCP、Apify、Alexa/COSMO 语音购物优化要求或用户上传数据，稳定完成标题、目标受众、五点、长描述、核心亮点、使用场景、Search Terms/ST、后台属性、Q&A 意图映射、关键词分配、父子体关键词矩阵、合规检查，并输出 Excel 或 HTML。  
Goal: help agents use ASINs, keyword libraries, competitors, reviews, Sorftime MCP, Apify, Alexa/COSMO voice-shopping requirements, or uploaded data to produce stable Amazon titles, bullet points, long descriptions, target audience, core selling points, use scenarios, Search Terms/ST, backend attributes, Q&A intent maps, keyword allocation, variation keyword matrices, compliance checks, and Excel or HTML deliverables.

## How To Load / 如何加载

1. 先读取 `SKILL.md`，它是完整工作流的主文件。  
   Read `SKILL.md` first. It is the source of truth for the workflow.
2. 按任务需要读取 references，不要一次性塞满上下文。  
   Read references only when needed:
   - `references/sorftime-mcp.md` for Sorftime MCP data workflows.
   - `references/apify-data-pipeline.md` for Apify/Amazon data collection.
   - `references/compliance-checklist.md` for baseline listing compliance.
   - `references/policy-overlay.md` for user-provided or marketplace-specific rule overrides.
   - `references/amazon-sensitive-words.md` for customer-facing sensitive word scanning.
   - `references/output-provenance-schema.md` for Excel/HTML outputs and strict keyword provenance.
   - `references/alexa-shopping-optimization.md` for Alexa/COSMO, voice-shopping, backend attribute, negative-constraint, and Q&A intent optimization.
3. 优先使用已连接的 MCP/API 数据，再要求用户上传文件。  
   Use connected MCP/API data before asking the user to upload files.
4. 如果没有可用数据连接器，只向用户索要缺失的 ASIN、产品、竞品、评论或关键词数据。  
   If no data connector is available, ask for only the missing ASIN, product, competitor, review, or keyword data.

## Universal Task Contract / 通用任务约定

当用户要求优化 Amazon Listing 时，先判断任务类型。  
When a user asks for Amazon Listing optimization, classify the job:

- `single_asin`: one sellable ASIN.
- `batch_asins`: multiple independent ASINs.
- `parent_child_asins`: a parent variation family with child ASINs.

然后按用户需要输出结果。  
Then produce the requested output:

- `关键词相关性预审` / keyword relevance pre-review, user confirmation questionnaire, `定位分析` / positioning analysis, title, target audience, style, five bullet points, long description, Search Terms/ST, embedded-keyword provenance table, keyword usage confirmation, scores, and compliance notes.
- Final one-shot Listing output must contain exactly these fields by default: Title, Target Audience, Style, Bullets, Long Description, Search Terms/ST, and `埋入词溯源表`. Add A+ modules, image copy, backend attributes, scoring, Excel, HTML, or variation matrices only when the user explicitly asks for those deliverables.
- Do not generate final Listing copy from product facts alone. A keyword library, analyzed keyword file, source export, or user-approved manual keyword table is required before outputting Title, Target Audience, Style, Bullets, Long Description, or ST.
- The title must answer what the product is, who it is for, and what the key highlight is.
- Do not include the brand name in the customer-facing title by default. Use the brand in the title only when the user explicitly asks for it, the marketplace/category template requires it, or there is a documented verified brand-search reason.
- Bullet logic must cover core selling points first, then product information such as features, size, audience, and scenario.
- Long description may expand to 10,000 characters for brand image and scenario depth. A+ analysis must provide module descriptions plus keywords/long-tail terms without repeating bullets.
- Search Terms/ST must stay within 250 characters, avoid repetition/plural stuffing/infringement, and exclude advertising words.
- Style must be 100 characters or fewer, including spaces and punctuation.
- Target audience is a flexible parallel Listing field. Output publishable `for ...` phrases first, not broad persona paragraphs. Include at least five `for ...` lines by default and add more when relevant and supported, such as `for women`, `for yourself`, `for mom`, `for daughter`, `for wife`, `for girlfriend`, `for friends`, `for wedding guest`, `for daily outfits`, `for office wear`, `for party wear`, `for date night`, `for photo shoots`, `for wedding looks`, `for vacation outfits`, or `for garden party looks`. Persona notes are optional supporting analysis only.
- When character totals are requested, count spaces and punctuation by module: title, target audience total, each target-audience line, style, bullets, long description, ST, A+ analysis/module copy, main/sub image copy, and total.
- Before writing final Listing copy, compare upstream keywords against current product facts and mark preliminary relevance as `High`, `Medium`, `Low`, or `None`. Output an Excel pre-review plus a user confirmation questionnaire when terms need approval. Generate final Listing copy only after the user confirms the keyword table and questionnaire.
- For fashion, jewelry, accessories, and outfit-driven products, bullets must cover styling details when supported: outfit pairing, daily/photo shoot/wedding/beach/date/going-out/party scenes, short hair/long hair/updo styling, flexible face-shape language, and the intended mood or atmosphere.
- Treat Reddit, Etsy, fashion-community, or trend-report notes as style/scenario insight sources, not product facts. Use them for nature-inspired styling, statement-piece pairing, metal-tone matching, garden party, summer/vacation, beach, date, party, hair, and outfit guidance; do not turn them into unconfirmed material, craft, flower-species, handmade, resin, enamel, ceramic, or sterling-silver claims.
- For variation families, include a parent-child keyword distribution matrix.
- For batch jobs, prefer Excel. For narrative research, prefer HTML.
- Every embedded keyword must be traceable to a source file/API/sheet/ASIN, user-confirmed product fact, or explicit manual seed. Include a Chinese `埋入词溯源表` provenance table in every final Listing output, including plain chat output.
- Do not run keyword scoring, keyword classification, or broad market-brand analysis inside this skill. Confirm analyzed keyword inputs only; route missing keyword analysis to the relevant keyword/search skill.
- For Alexa/COSMO or voice-shopping work, prioritize verified entity attributes, negative constraints, backend attribute fill rate, and voice-style Q&A intent mapping over keyword density.

Before generating customer-facing copy, run the intake gate in `SKILL.md`: confirm known facts, list missing facts, flag risky claims, evaluate data sufficiency, and ask the user what to do next. Do not draft final Listing copy from incomplete product facts or without a confirmed keyword table. Do not put unresolved questions into Excel; discuss them with the user first.

If the user provides product information without a keyword table, stop at guidance. Ask them to upload or paste one of these: analyzed keyword workbook from the keyword-analysis skill, SellerSprite/Helium 10/Jungle Scout/ABA/SQP/Ads/Sorftime/Apify keyword export, or a manual table with keyword, source, relevance, and intended destination.

When the user uploads SellerSprite or similar exports, evaluate whether the current exports are enough for the requested deliverable. If useful but incomplete, guide the user to export the next needed reports such as Keyword Mining, Reverse ASIN, Search Results, parent-child variation mapping, review exports, ABA/Search Query Performance, ad search term reports, or current listing fields.

生成面向买家的文案前，必须先执行 `SKILL.md` 里的资料检查门槛：确认已知事实、列出缺失信息、标出高风险声明，并询问用户下一步。产品事实不完整时，不要直接生成最终 Listing，除非用户明确要求先出 provisional 草稿。

## Tool-Specific Use / 不同工具使用方式

- Codex Skills：把整个文件夹安装到 skills 目录。  
  Codex Skills: install the whole folder under the skills directory.
- Claude Code：把本文件夹放进仓库，并要求 Claude follow `amazon-listing-optimizer/AGENTS.md`。  
  Claude Code: place this folder in the repository and ask Claude to follow `amazon-listing-optimizer/AGENTS.md`.
- Cursor：把本文件夹加入项目，在 Project Rules 中引用 `AGENTS.md` 或 `SKILL.md`。  
  Cursor: add this folder to the project and reference `AGENTS.md` or `SKILL.md` from project rules.
- Trae、QoderWork、CodeBuddy 等：让工具索引或附加整个文件夹，并以 `AGENTS.md` 作为入口说明。  
  Trae, QoderWork, CodeBuddy, and similar tools: attach or index this folder, then use `AGENTS.md` as the instruction entrypoint.

安装或分享时不要拆散文件夹，`SKILL.md` 依赖 `references/` 里的文件。  
Do not split this folder during installation. `SKILL.md` depends on the files in `references/`.
