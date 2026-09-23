# front-UI-check

`figma-ui-reconstruction` 是一个 Codex Skill：在现有前端项目中还原 Figma 应用页面，并通过真实运行页面的截图检查还原效果。它适用于新页面实现、已有页面与设计稿对齐，以及按要求拆分页面模块的工作。

## 使用前准备

- 一个可访问的 Figma 设计链接，最好直接指向完整页面的节点；如果设计需要授权，确保 Codex 能读取它。
- 一个可运行的前端项目，以及需要实现或检查的页面路由。
- 明确目标视口尺寸；如有响应式、交互或模块拆分要求，也在任务中说明。

## 安装

将整个仓库放进 Codex 的 Skill 目录，保持 [`SKILL.md`](SKILL.md)、`agents/` 和 `references/` 的相对位置。只克隆到普通工作目录不会自动让 Codex 发现这个 Skill。

在 Windows PowerShell 中安装给当前用户：

```powershell
New-Item -ItemType Directory -Force "$HOME/.agents/skills" | Out-Null
git clone https://github.com/KentLin0/front-UI-check.git "$HOME/.agents/skills/figma-ui-reconstruction"
```

在 macOS 或 Linux 中：

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/KentLin0/front-UI-check.git ~/.agents/skills/figma-ui-reconstruction
```

如果只想在某个前端项目中使用，也可以把整个仓库放在 `<项目根目录>/.agents/skills/figma-ui-reconstruction/`。安装后在 Codex 中输入 `$` 查找 `figma-ui-reconstruction`；如果没有出现，重启 Codex 再检查。安装位置和发现机制见 [Codex 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。

## 如何使用

在**目标前端项目**中打开 Codex，发送带有 Figma 链接和验收条件的任务。显式调用示例：

```text
使用 $figma-ui-reconstruction，还原这个 Figma 页面：<Figma 页面链接或节点链接>。
在当前前端项目中实现，目标路由是 /dashboard，按 1440×900 视口检查。
保留设计中的完整内容、控件和状态，并提供运行页面与设计稿的截图对照。
```

已有页面的视觉修正也可以这样描述：

```text
使用 $figma-ui-reconstruction，对照 <Figma 节点链接> 检查当前项目的 /dashboard。
修正有证据支持的布局和细节差异，按 1440×900 视口验证运行页面。
```

当任务明确涉及 Figma 页面还原或对齐时，Codex 也可以根据 Skill 描述自动选用它；写出 `$figma-ui-reconstruction` 可明确指定。需要多个模块分别实现时，请在任务里指出要拆分的页面区域。

Skill 会先确定完整的 Figma 对照范围，再检查项目现有组件与样式、实现页面，并在实际运行的目标路由上截图复核。交付内容包括代码改动、相关检查结果、Figma 参考图、运行页面截图、并排对照图和简短的检查记录；若要求模块拆分，还会附上对应的模块说明。截图应注明 Figma 节点、最终页面 URL、视口、设备像素比和采集时间。

## 文件说明

- [`SKILL.md`](SKILL.md)：Skill 的触发条件、实施流程与交付要求。
- [`references/module-contract.md`](references/module-contract.md)：需要模块化实现时使用的说明模板。
- [`references/visual-review.md`](references/visual-review.md)：截图复核与争议问题的验证方法。
- [`agents/openai.yaml`](agents/openai.yaml)：Codex 中显示的名称、简介和默认提示词。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
