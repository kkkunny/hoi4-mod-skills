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
- `SKILL.md` 只放要点，穷举参数与边缘情况放 `skills/hoi4-mod/references/`；技能内容必须与 `docs/` 原文一致，不引入未经验证的机制。
- `docs/` 与 `skills/hoi4-mod/references/` 内容相同，改任一侧必须同步另一侧。
- 文档按受众分流：agent 用 → 本文件；人类用 → README.md，两者不重复。
- HOI4 侧硬性规范（编码、覆盖策略、error.log 等）以 `skills/hoi4-mod/SKILL.md` 为准，勿在此重复维护。
