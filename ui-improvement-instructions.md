% UI Improvement Instructions for Portfolio Website

任务目标（一句话）
- 使站点视觉更高级、简约且一致，重点调整：排版、间距、层级、圆角与阴影，并本地化 CTA 文案与修复简历下载链接。

总体要求（必须遵守）
- 不改动文字内容（除本地化按钮/导航短文案）。
- 只修改 `index.html` 与 `style.css`（必要时可微调 `script.js` 用于无障碍属性），保持最小改动。
- 提交前在本地浏览器检查桌面/平板/移动视图。
- 提交分支：`fix/ui-polish`，commit message：`ui: polish spacing, typography, radius, shadows, localize CTAs`。

按顺序执行的具体步骤（精确可执行）

1) 高优先（必须做）
  - 在 `index.html` 的 `<head>` 中加入字体引入（放在现有 preconnect 后）：
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">

  - 在 `style.css` 的 `:root` 中替换或新增以下变量（精确值）：
    :root {
      --radius-lg: 16px;
      --radius-md: 12px;
      --radius-sm: 10px;
      --shadow: 0 18px 40px rgba(17,24,39,0.06);
      --shadow-hover: 0 20px 50px rgba(53,76,160,0.12);
    }

  - 基础排版与行高（在 `style.css` 中加入）：
    body { font-family: "Inter", "Noto Sans SC", system-ui, -apple-system, "Segoe UI", sans-serif; line-height: 1.6; }
    .hero h1 { line-height: 1.04; font-weight: 700; }
    .hero-summary { max-width: 560px; }

  - 缩减 section 顶底间距：
    .section { padding: 100px 0; }
    @media (max-width: 640px) { .section { padding: 64px 0; } }

  - 本地化 CTA 文案（修改 `index.html`）：
    - `<a class="button button-primary" href="#projects">View Projects</a>` -> `<a class="button button-primary" href="#projects">查看项目</a>`
    - `<a class="button button-secondary" href="#contact">Contact Me</a>` -> `<a class="button button-secondary" href="#contact">联系我</a>`

  - 为 CV 添加 `download` 属性（若文件名固定）：
    `<a class="button button-secondary" href="cv.pdf" download>查看 PDF 简历</a>`

  - 更新 brand 样式：
    .brand { font-size: 1.12rem; font-weight: 700; }

2) 中优先（样式一致性）
  - 统一卡片圆角与内边距（在 `style.css` 中）：
    .portrait-card, .timeline-card, .experience-card, .project-card, .skill-card, .contact-panel {
      border-radius: var(--radius-md);
      padding: 22px;
    }

  - 主/次按钮样式调整：
    .button-primary { box-shadow: 0 14px 36px rgba(34,58,120,0.16); }
    .button-secondary { border: 1px solid rgba(17,24,39,0.06); background: rgba(255,255,255,0.72); box-shadow: none; }

  - 将 hero 文本 max-width 从 620px 改为 560px（已在高优先中给出）。

3) 低优先（打磨）
  - 细化背景光斑的透明度与尺寸以降低饱和感（可在 CSS 中微调 `.hero-bg-left` / `.hero-bg-right`）。
  - 检查 `--muted` 与背景对比度；若不足则微调颜色深度以满足可访问性（建议对比 >= 4.5:1 对正文）。
  - 若有真实 GitHub / LinkedIn 链接，替换占位并取消 muted 样式；若无，移除“TODO”或保留但去掉突出显示。

精确可粘贴的代码片段（供实现者直接粘贴）

-- index.html 插入（放在现有 preconnect 后）:
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Noto+Sans+SC:wght@400;700&display=swap" rel="stylesheet">

-- style.css 中替换或追加 :root 与基础样式片段:
:root {
  --radius-lg: 16px;
  --radius-md: 12px;
  --radius-sm: 10px;
  --shadow: 0 18px 40px rgba(17,24,39,0.06);
  --shadow-hover: 0 20px 50px rgba(53,76,160,0.12);
}

body {
  font-family: "Inter", "Noto Sans SC", system-ui, -apple-system, "Segoe UI", sans-serif;
  line-height: 1.6;
}

.section { padding: 100px 0; }

@media (max-width: 640px) { .section { padding: 64px 0; } }

.hero h1 { line-height: 1.04; font-weight: 700; }
.hero-summary { max-width: 560px; }

.brand { font-size: 1.12rem; font-weight: 700; }

.portrait-card,
.timeline-card,
.experience-card,
.project-card,
.skill-card,
.contact-panel {
  border-radius: var(--radius-md);
  padding: 22px;
}

.button-primary { box-shadow: 0 14px 36px rgba(34,58,120,0.16); }
.button-secondary { border: 1px solid rgba(17,24,39,0.06); background: rgba(255,255,255,0.72); box-shadow: none; }

质量检查（PR/提交前必须执行）
- 在本地打开 `index.html`，检查 Desktop 1366px、Tablet 820px、Mobile 375px。确认：
  - 首屏视觉不再过度拉伸（h1、hero-summary 行高与宽度良好）。
  - 导航与 CTA 已本地化且可见。  
  - 卡片圆角与阴影一致。  
  - `cv.pdf` 点击后触发下载或按预期打开。  
  - 控制台无 JS 错误（`script.js` 功能仍正常）。

提交规范
- 新分支：`fix/ui-polish`
- Commit message：`ui: polish spacing, typography, radius, shadows, localize CTAs`
- 在 PR 描述中列出变更要点（字体、间距、圆角、CTA 本地化、CV 下载）。

需要确认的内容（实现前请提问）
- 是否允许引入 Google Fonts？（默认已包含）
- `cv.pdf` 文件名是否为最终名称，是否希望强制下载或在新 tab 打开？
- 是否提供 GitHub / LinkedIn 链接以替换占位？

