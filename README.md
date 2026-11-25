# Editorial Review · 長文協作審查

用於 Codex 的長文編審 Skill。六個角色分三批工作，從同一版稿件獨立檢查讀者理解、段落結構與論證，最後由總編輯提出修訂方案。

## 安裝

在 Codex 使用 Skill 安裝器，從 `lokchonmou/editorial-review` 安裝 `skills/editorial-review`。也可將該目錄完整複製到專案的 `.agents/skills/`，再開啟新的 Codex 對話。

## 使用

```text
用 $editorial-review 審閱 report.md。
Target Audience 是未參與項目、懂一般研究方法的非工程背景評判。
請找出正文沒有建立的概念與論點，附反向大綱及優先修訂方案。
```

| 批次 | 角色 |
| --- | --- |
| 外部讀者 | Cold Reader、Logic Reviewer、Naive Reader |
| 內部論證 | Defender、Challenger |
| 整合 | Chief Editor |

主代理按角色提供資料，審閱者先各自回報。若環境缺少對話繼承控制，Skill 會標明採用單代理審查。對話隔離的驗收方式及檔案存取限制見[執行環境說明](skills/editorial-review/references/runtime.md)。角色細節與寫作方法見[審閱規格](skills/editorial-review/references/review.md)。
