# AGENTS.md

本仓库是 HOI4（钢铁雄心 4）mod 制作的 Agent Skills 库，入口技能为 `skills/hoi4-mod`。

## 参考资料优先级

- `docs/` 是 modding wiki 的离线快照。**优先查离线**；仅当内容涉及比快照更新的游戏版本（如使用者明说是新版本内容），或本地没有的主题时，才读在线 wiki。
- 游戏 wiki：https://hoi4.paradoxwikis.com/Hearts_of_Iron_4_Wiki
  镜像：https://cooderatom.github.io/hoi4-zh-wiki/Hearts_of_Iron_4_Wiki
- mod 制作 wiki：https://hoi4.paradoxwikis.com/Mod_structure
  镜像：https://cooderatom.github.io/hoi4-zh-wiki/Mod_structure.html
- 镜像规则：把 `hoi4.paradoxwikis.com` 换成 `cooderatom.github.io/hoi4-zh-wiki`；镜像文章 URL 以 `.html` 结尾。

## 约定

- 技能即 `skills/<id>/SKILL.md`：`name` 与目录名一致；`description` 决定何时触发，写清“何时使用 + 覆盖内容”；交叉引用用 `[[skill-id]]`，增删改名技能须同步主技能索引。
- 技能内容必须与 `docs/` 原文一致，不引入未经验证的机制。
- `docs/` 与 `skills/hoi4-mod/references/` 内容相同，改任一侧必须同步另一侧。
- 文档按受众分流：agent 用 → 本文件；人类用 → README.md，两者不重复。
- HOI4 侧硬性规范（编码、覆盖策略、error.log 等）以 `skills/hoi4-mod/SKILL.md` 为准，勿在此重复维护。

## 技能正文与 references 的边界

`SKILL.md` 与 `references/` 各自只回答一个问题，不得越界：

- **`SKILL.md` = “用哪个系统、在哪写、大体怎么做”**：技能定位、文件位置与命名规则（同名覆写 / 新文件新增）、一个最小骨架、总体规则要点、致命陷阱、路由。
- **`references/` = “每件事具体怎么做”**：完整代码模板与变体、全字段/参数枚举、常用清单、边缘情况、DLC/版本差异、工具操作细节。

一条内容能否留在正文，需同时满足以下三条（否则下沉 references）：

1. **结构性**：不知道会导致文件位置、命名或加载方式错误；
2. **致命性**：不遵守会崩溃或该系统完全失效（仅崩溃级；“显示不对/数值不对”级移入 references）；
3. **路由性**：影响选择哪个文件或哪个子技能的判断。

其他正文硬性要求：

- 每个技能正文最多保留一个「最小可运行骨架」代码块（约 10–15 行），其余示例、长模板一律放 references。
- 正文中不重复 references 已有内容。同一知识点只在一处完整展开（references 或某一篇正文），其他技能一行带过并用 `[[skill-id]]` 引流，不得复制完整段落、模板或清单。
- 路由写法统一为 `references/<文件名>.md`（相对 `skills/hoi4-mod/` 目录），不写仓库相对路径。
- 规模参考：子技能正文 50–100 行，主技能不超过约 120 行。
- 正文与 references 均为英文（`docs/` 快照的语言）。
