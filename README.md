# claude_code_practice

以 Claude Code（在 Cursor 中運作的高自主程式助理）練習代理式開發工作流與最佳實務。

## 課程大綱

- **Introduction** — Video · 4 mins
- **What is Claude Code?** — Video · 8 mins
- **Course Notes** — Reading · 1 min
- **Setup & Codebase Understanding** — Video · 14 mins
- **Adding Features** — Video · 17 mins
- **Testing, Error Debugging and Code Refactoring** — Video · 12 mins
- **Adding Multiple Features Simultaneously** — Video · 11 mins
- **Exploring Github Integration & Hooks** — Video · 10 mins
- **Refactoring a Jupyter Notebook & Creating a Dashboard** — Video · 12 mins
- **Creating Web App based on a Figma Mockup** — Video · 9 mins
- **Conclusion** — Video · 1 min
- **Prompts & Summaries of Lessons** — Reading · 10 mins

---

## What is Claude Code?

以下以實作「互動視覺化」示範 Claude Code 的代理式流程：從高層目標 → 規劃 → 編碼 → 執行/回饋 → 修正。

### 目標

- 在 Claude Code 內輸入的指令：
  - `make a cool visualization for me im just getting started`

### Claude 的規劃（自動生成的待辦）

- Update Todos：
  - Create an interactive visualization with animated elements
    - Set up HTML structure with canvas or SVG
    - Add CSS styling for visual appeal
    - Implement JavaScript for interactivity and animation

### 實作輸出

- 生成檔案：`visualization.html`（約 210 行）
- 位置：`/home/rd8312/projects/claude_code_practice/visualization.html`
- 技術：HTML5 Canvas、玻璃擬態 UI、平滑動畫

### 成品功能

- 粒子系統與三種動畫模式：`spiral`、`explosion`、`gravity`
- 滑鼠互動：跟隨游標、點擊產生粒子爆炸
- 控制面板：Pause/Play、Change Pattern、Random Colors
- 多組配色與動態發光漸層

### 嘗試預覽與執行

- 在 Claude Code 內輸入：
   - `> open it in the browser` 嘗試直接以瀏覽器開啟。


