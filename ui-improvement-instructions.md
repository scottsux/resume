 % Remaining UI Changes (only the tasks still required)
 
 任务目标
 - 对已应用的改动进行收尾：本地化剩余交互文本、修正少数样式不一致、处理社媒占位。
 
 总体要求
 - 仅应用下列未完成项；不要重复已完成的改动（字体、:root、CTA 本地化、CV download、section padding 等已完成）。
 - 修改文件：`index.html`、`style.css`、`script.js`（仅脚本本地化相关部分）。
 - 提交分支：`fix/ui-polish`，commit message：`ui: finish localization and polish small UI inconsistencies`。
 
 未完成项（按优先级）
 
 1) 高优先（必须做）
   - 本地化项目展开按钮并让脚本使用可配置文本：
     - HTML：为每个 `.project-toggle` 按钮添加 `data-open-text="详情"` 与 `data-close-text="关闭"`，并将按钮初始文本设为 open 文本（例如 `详情`）。
     - JS：修改 `script.js` 中的 toggle 处理，改为读取 `dataset.openText` / `dataset.closeText`，避免硬编码英文。见下方代码片段。
 
   - 将 `portrait-media` 的圆角从固定值（26px）改为变量：`border-radius: var(--radius-md);`，以保证圆角一致性。
 
 2) 中优先
   - 本地化或移除 hero eyebrow 英文语句（将 `Computer Science & Machine Learning Graduate Student` 改为中文或双语短句，例如：`计算机科学 · 机器学习 研究生`）。
 
   - 处理 Contact 中的 TODO：若已有 GitHub / LinkedIn 链接，请替换占位并移除 `contact-item-muted`，若没有则把显示文字改为不显眼的占位（例如 `暂未提供`），或移除该条目。
 
 3) 低优先（可选）
   - 轻微调整 `.hero-bg-left` / `.hero-bg-right` 的透明度与尺寸以降低背景光斑饱和度（仅视觉打磨）。
   - 校验 `--muted` 与背景的对比度，若低于可访问性阈值（建议 >= 4.5:1）请略微加深 `--muted`。
 
 精确可粘贴的代码片段（直接替换/插入）
 
 HTML：在每个项目卡的按钮处替换为（示例）：
 <button
   class="project-toggle"
   type="button"
   aria-expanded="false"
   aria-label="展开项目详情"
   data-project-toggle
   data-open-text="详情"
   data-close-text="关闭"
 >
   详情
 </button>
 
 JS：替换 `script.js` 中项目展开处理器为：
 projectToggles.forEach((toggle) => {
   toggle.addEventListener("click", () => {
     const card = toggle.closest("[data-project-card]");
     const details = card?.querySelector(".project-details");
     if (!details) return;
 
     const isExpanded = toggle.getAttribute("aria-expanded") === "true";
     const openText = toggle.dataset.openText ?? "Details";
     const closeText = toggle.dataset.closeText ?? "Close";
 
     toggle.setAttribute("aria-expanded", String(!isExpanded));
     toggle.textContent = !isExpanded ? closeText : openText;
     details.hidden = isExpanded;
   });
 });
 
 CSS：替换 `portrait-media` 圆角为变量（在 `style.css` 中）：
 .portrait-media {
   margin: 0;
   overflow: hidden;
   border-radius: var(--radius-md);
   aspect-ratio: 0.74;
   background: linear-gradient(180deg, rgba(77, 104, 255, 0.14), rgba(255, 255, 255, 0.35));
 }
 
 小测试清单（变更后请验证）
 - 点击任意项目卡的 `详情` 按钮，能够展开/收起并显示本地化文本（`详情` / `关闭`）；控制台无错误。  
 - 头像区域圆角与其他卡片圆角一致（使用 `var(--radius-md)`）。
 - hero eyebrow 已本地化或改为双语；Contact 中不再出现 `TODO` 明显占位。  
 
 提交规范
 - 分支：`fix/ui-polish`
 - commit message：`ui: finish localization and polish small UI inconsistencies`
 
 如果你同意，我现在可以应用这些三处补丁（HTML、CSS、JS）并创建分支与提交。回复“现在修改”即可。

附加：用户反馈的尺寸与间距调整（已请求写入）

背景
- 用户反馈：页面整体字体显得偏大，同时一屏能看到的内容过多，视觉上“满满当当”。需要通过缩小整体字号、适度减少标题体量、增加垂直留白并在大屏将项目从三列改为两列，来改善页面呼吸感与层次。

目标
- 让页面看起来更“高级”与宽松：略微缩小根字号；减小 hero 标题体量；增加 section 垂直留白；在大屏将项目卡改为两列展示；加大卡片内边距与行距以提升阅读体验。

变更（精确可粘贴的 CSS 片段）
- 在 `style.css` 末尾或合适位置追加以下规则：

/* --- 用户请求：调整字号与间距 --- */
html {
  /* 将根字号微调为 15px，降低信息密度 */
  font-size: 15px;
}

.hero h1 {
  /* 减小主标题体量，保持响应式 */
  font-size: clamp(2.8rem, 7vw, 5rem);
  line-height: 1.04;
}

.section {
  /* 增加垂直留白以减少拥挤感 */
  padding: 120px 0;
}

.hero-grid {
  gap: 64px; /* 增大左右模块间距 */
}

.projects-grid {
  gap: 32px;
  grid-template-columns: repeat(2, minmax(0, 1fr)); /* 大屏改为两列 */
}

.project-card,
.skill-card,
.timeline-card,
.portrait-card {
  padding: 28px; /* 增加卡片内边距提升可读性 */
}

.project-summary,
.project-stack,
.lead {
  line-height: 1.6; /* 更舒适的行高 */
}

@media (max-width: 1080px) {
  .projects-grid { grid-template-columns: 1fr; gap: 22px; }
  .section { padding: 96px 0; }
}

@media (max-width: 640px) {
  .section { padding: 64px 0; }
  /* 保持移动端文本可读性，避免压缩 */
  html { font-size: 15px; }
}

小测试清单（变更后请验证）
- Desktop（1366px）：
  - 页面垂直留白增加，首屏不再过度拥挤；
  - Projects 显示为两列，卡片大小与间距更舒适；
  - hero 标题体量明显降低但仍具表现力；

- Tablet（820px） & Mobile（375px）：
  - 布局回落为单列，间距与内边距在视觉与可读性上保持合理；
  - 控制台无 JS 报错，交互仍然正常。

实现说明
- 这些改动是视觉层面的微调，影响整个页面的节奏与信息密度。请在合并前在本地浏览器进行对比测试（改动前后截图或直接切换分支）。



