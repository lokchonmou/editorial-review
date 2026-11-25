# 長文協作審查 · 多代理編審<br>Editorial Review — Multi-Agent Writing Auditor

## 為甚麼開發這個 Skill？<br>Why I Built This Skill

目前我使用 AI 撰寫長篇文章的工作流程如下：<br>
Here is how I currently use AI to write long-form articles:

1. 與 AI 深入討論觀點、背景及論據。<br>
   Discuss the ideas, background, and supporting evidence with AI in depth.
2. 生成細化至三級標題的文章大綱。<br>
   Generate an outline with up to three levels of headings.
3. 人工審查大綱及確認論述方向。<br>
   Review the outline myself and confirm the direction of the argument.
4. 由 AI 根據大綱撰寫全文。<br>
   Have AI draft the full article from the outline.

然而，問題往往出現在第四步：AI 容易將各級標題視為獨立的內容填充任務，而不是一條完整的論證鏈。<br>
Problems tend to arise in step four, when AI treats each heading as a separate writing task and loses sight of how the sections build a single argument.

因此，即使每個小節獨立閱讀都沒有問題，整篇文章仍可能出現以下缺陷：<br>
Each subsection may read well on its own, yet the article as a whole can still have these problems:

- **概念突現（Concept Introduction）**：突然引入一個專業術語，但前文沒有任何鋪墊或定義。<br>
  **Abrupt introduction of concepts:** A technical term appears without any prior explanation or definition.
- **論述斷層（Logical Gap）**：由觀點 A 跳到觀點 C，缺少中間推論 B。<br>
  **Logical gaps:** The argument jumps from point A to point C without explaining the intermediate step B.
- **局部合理、全局失衡（Local vs. Global Coherence）**：每個小節都有道理，但組合起來卻未能構成完整的論述。<br>
  **Local vs. global coherence:** Each subsection makes sense on its own, yet the sections fail to form a coherent argument.
- **語境流失（Context Loss）**：之前與 AI 討論過的重要觀點，在正式寫作時被遺漏或弱化。<br>
  **Context loss:** Important ideas discussed with AI earlier are omitted or given less weight in the draft.
- **虛假連貫性（False Coherence）**：AI 使用「因此」、「綜上所述」、「基於上述分析」等連接詞，掩飾實際上並不存在的邏輯關係。<br>
  **False coherence:** AI uses transitions such as “therefore,” “in summary,” and “based on the analysis above” to imply logical connections the text has yet to establish.

**一份結構完整的大綱，並不保證能產出一篇論述連貫的文章。**<br>
**A well-structured outline alone cannot guarantee a coherent argument.**

## 設計理念<br>Design Rationale

另一個問題是，當作者長時間投入某個研究或項目，往往會將部分專業知識、背景資訊及隱含假設視為理所當然。<br>
When authors spend a long time on a study or project, they often take some specialist knowledge, background information, and implicit assumptions for granted.

我在多年指導學生參與創新科技比賽的過程中，發現非工程背景的老師雖然未必具備相關技術知識，卻經常能憑藉良好的邏輯思維及敘事能力，指出工程背景的老師和學生所忽略的問題。<br>
Over years of coaching students for technology innovation competitions, I’ve found that teachers from outside engineering often use their reasoning and storytelling skills to spot issues that engineering teachers and students overlook, even when they are unfamiliar with the technology.

這些問題不一定源於技術錯誤，而是源於知識的詛咒（Curse of Knowledge）、專家盲點（Expert Blind Spot），甚至是先有方案、再尋找問題的事後合理化。<br>
Even when the technical details are sound, these issues can stem from the curse of knowledge, expert blind spots, or post hoc rationalization, when someone starts with a solution and then looks for a problem to justify it.

因此，我開發了這個 Skill，透過多代理審查（Multi-Agent Review）及語境隔離（Context Isolation），模擬不同知識背景、不同立場及不同審查職能的讀者，從多個角度審視同一篇文章。<br>
I built this Skill to review the same article from several perspectives, using multi-agent review and context isolation to give readers different backgrounds, positions, and review responsibilities.

## 六個專職 Agents<br>Six Specialist Agents

| Agent              | 角色定位<br>Role | 核心職責<br>Core Responsibilities |
| ------------------ | -------------- | ------------------------------------------------------------ |
| **Defender**       | 正向論證者<br>Constructive advocate | 理解作者原意，識別有價值的觀點，保留及強化合理論證，而非盲目認同作者。<br>Understand the author’s intent, identify valuable ideas, and preserve and strengthen sound arguments while exercising independent judgment. |
| **Challenger**     | 反向論證者<br>Critical challenger | 主動質疑文章的觀點、證據及假設，尋找反例、論證漏洞及確認偏誤。<br>Question the article’s claims, evidence, and assumptions to find counterexamples, weaknesses in the reasoning, and confirmation bias. |
| **Cold Reader**    | 首次閱讀者<br>First-time reader | 未曾參與原始討論，以首次閱讀文章的角度，檢查術語突現、背景資訊缺失及未經交代的概念。<br>Read without access to the original discussion and flag abruptly introduced terms, missing background, and unexplained concepts. |
| **Logic Reviewer** | 邏輯流程審查員<br>Argument flow reviewer | 不預先閱讀原始大綱，直接根據文章建立 Reverse Outline，檢查論證鏈、因果關係、資訊排序及段落銜接。<br>Build a reverse outline from the article before seeing the original outline, then check the argument chain, causal links, information order, and transitions between paragraphs. |
| **Naive Reviewer** | 非領域專家讀者<br>Reader from outside the field | 模擬缺乏相關領域先備知識、但具備良好邏輯及批判思考能力的讀者，識別專家盲點、隱含假設及敘事障礙。<br>Simulate a reader with strong reasoning and critical thinking skills but limited prior knowledge of the field to identify expert blind spots, implicit assumptions, and obstacles in the narrative. |
| **Chief Editor**   | 主審編輯<br>Lead editor | 整合五個獨立 Reviewer 的審查結果，處理重複或互相矛盾的意見，判斷問題的嚴重程度，制定修訂策略，同時確保文章不偏離作者原意。<br>Synthesize the five independent reviews, reconcile duplicate or conflicting feedback, assess the severity of issues, and plan revisions that preserve the author’s intent. |

## Context Isolation：不同 Agent，不同資訊權限<br>Context Isolation and What Each Agent Can See

這個 Skill 的核心並不只是增加 Agent 的數量，而是有意識地控制每個 Agent 所能獲取的資訊。<br>
The design centers on deliberately controlling what information each agent receives so that adding agents brings different perspectives.

如果所有 Agent 都掌握相同的背景資訊，它們便可能共享相同的認知盲點，甚至自行補全文章中原本缺失的推論。<br>
If every agent has the same background information, they can share the same blind spots and fill in reasoning the article leaves unstated.

因此，各 Agent 採用不同的 Context Access Policy：<br>
Each agent follows a different context access policy:

| Agent          | 原始討論<br>Original Discussion | 原始大綱<br>Original Outline | 文章初稿<br>Draft | 其他審查意見<br>Other Reviewers’ Feedback |
| -------------- | -------- | -------- | -------- | ------------ |
| Defender       | ✅        | ✅        | ✅        | ❌            |
| Challenger     | ✅        | ✅        | ✅        | ❌            |
| Cold Reader    | ❌        | ❌        | ✅        | ❌            |
| Logic Reviewer | ❌        | ❌        | ✅        | ❌            |
| Naive Reviewer | ❌        | ❌        | ✅        | ❌            |
| Chief Editor   | ✅        | ✅        | ✅        | ✅            |

所有 Reviewer 均以同一版本的文章進行獨立審查，避免受其他 Agent 的意見影響。<br>
All reviewers independently read the same version of the article so that other agents’ feedback does not influence their findings.

最後才由 Chief Editor 統一整合及裁決。<br>
The Chief Editor then brings the findings together and resolves disagreements.

## 目標讀者<br>Target Audience

不同文章的目標讀者具有不同的知識背景及閱讀目的。<br>
Readers come to different kinds of writing with different background knowledge and goals.

因此，Skill 將 Target Audience 設為可配置的輸入參數。<br>
The Skill therefore lets you specify the Target Audience as an input.

例如：<br>
Examples:

- 學術論文：相關或其他研究領域的學者。<br>
  Academic paper: Researchers in related or other fields.
- 創新科技比賽報告：非工程背景的評判。<br>
  Technology innovation competition report: Judges without an engineering background.
- 學校政策文件：未曾參與政策制定的教師。<br>
  School policy document: Teachers who did not take part in developing the policy.
- 技術文件：首次接觸該系統的工程師。<br>
  Technical documentation: Engineers encountering the system for the first time.

審查的目的並非要求所有文章都能讓任何人理解，而是確保文章為其目標讀者提供足夠的資訊、合理的論證及清晰的閱讀路徑。<br>
The review is tailored to the intended audience and asks whether the article gives those readers enough information, sound reasoning, and a clear path through the text.

## 核心目標<br>Main Goal

Editorial Review 並不是另一個 AI 文章生成或文字潤飾工具，而是一個面向長篇文章的品質保證系統（Quality Assurance System）。<br>
Editorial Review goes beyond AI drafting and copyediting by providing quality assurance for long-form writing.

它不單關注文章是否寫得流暢，更關注：<br>
Alongside readability, it asks:

- 作者真正想表達的內容，有沒有完整傳達？<br>
  Does the article fully convey what the author intended to say?
- 文章的論證是否成立？<br>
  Does the argument hold up?
- 讀者能否在沒有參與前期討論的情況下理解文章？<br>
  Can readers understand the article without taking part in the earlier discussions?
- 每個段落是否對整體論述具有實質貢獻？<br>
  Does each paragraph make a substantive contribution to the overall argument?
- 文章是否具備真正的連貫性，而非僅僅依靠語言上的流暢？<br>
  Do the ideas genuinely connect, even if the prose already reads smoothly?

**目標是讓 AI 生成的文章思路更清晰、論證更扎實、表達更到位，並提升文字品質。**<br>
**The goal is not merely to make AI-generated writing sound better, but to make it think better, argue better, and communicate better.**
