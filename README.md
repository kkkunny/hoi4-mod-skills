# hoi4-mod-skills

让 AI agent 学会制作《钢铁雄心 4》（Hearts of Iron IV）模组的技能包 —— 包含 20 个 Agent Skills，覆盖从零创建 mod 到脚本、国策、事件、地图、界面的完整流程。

## 这是什么

本仓库把 HOI4 的 mod 制作知识整理成 20 个**技能**，供支持 Agent Skills 的 AI 编码助手使用（如 OpenCode、Claude Code）。安装之后，你只需要用自然语言描述需求，agent 会自动加载对应技能来完成工作——它知道该改哪些目录、文件怎么写、常见坑在哪里。

- 1 个主技能负责总览与调度，19 个领域子技能负责具体实现；
- 附带 49 篇 HOI4 modding wiki 离线文档，不联网也能查资料；
- 覆盖从“新建国家”到“地图与界面”的全部常见 mod 需求。

## 覆盖内容

| 分类 | 技能 | 说明 |
|---|---|---|
| 入口 | `hoi4-mod` | mod 结构、文件布局与制作工作流的总览，指引其它所有技能 |
| 国家与内容 | `hoi4-country` | 新建国家：tag、国旗、初始设置 |
| | `hoi4-focus` | 国策树、共同国策、持续国策 |
| | `hoi4-event` | 国家 / 新闻 / 州事件 |
| | `hoi4-decision` | 决议与任务 |
| | `hoi4-idea` | 国家精神、法案、设计商 |
| | `hoi4-character` | 领袖、顾问、将领 |
| | `hoi4-ideology` | 意识形态与政党 |
| | `hoi4-autonomy` | 附庸与自治等级 |
| | `hoi4-bookmark` | 开局书签 |
| 脚本 | `hoi4-script` | effects / triggers / scopes 脚本核心 |
| | `hoi4-modifier` | 修正（修正器）|
| | `hoi4-onaction` | on_actions 钩子 |
| | `hoi4-define` | 游戏全局常量（defines）|
| 军事与科技 | `hoi4-tech` | 科技、师编制、装备、OOB |
| 地图与资产 | `hoi4-map` | 地图编辑与州历史 |
| | `hoi4-gui` | 贴图、界面、scripted GUI |
| | `hoi4-assets` | 本地化、音频、gfx 总览 |
| 其它 | `hoi4-ai` | AI 行为与策略计划 |
| | `hoi4-trouble` | 错误排查与调试 |

## 安装

1. 准备一个支持 Agent Skills 的 AI 编码 agent。
2. 把 `skills/` 下的技能目录放进 agent 的技能目录：

| 工具 | 全局目录 | 项目目录 |
|---|---|---|
| OpenCode | `~/.config/opencode/skills/` | `<项目>/.opencode/skills/` |
| Claude Code | `~/.claude/skills/` | `<项目>/.claude/skills/` |

```bash
# 复制（以 OpenCode 全局安装为例）
cp -r skills/* ~/.config/opencode/skills/

# 或软链接，方便跟随仓库更新
ln -s "$PWD"/skills/* ~/.config/opencode/skills/
```

3. 重启 agent（或新开一轮对话），技能即可被加载。

## 使用

安装后无需手动指定技能，直接描述需求即可：

- “帮我从零创建一个 HOI4 mod，叫 XXX”
- “给德国写一棵国策树，包含工业线、军事线和重建线”
- “写一个满足……条件时触发的新闻事件”
- “我的 mod 加载报错了，帮我分析 error.log”

本机装有 HOI4 时，agent 还可以对照原版游戏文件，产出更准确。

## 仓库内容

- `skills/` — 技能本体：每个目录一个技能，入口为 `SKILL.md`
- `docs/` — HOI4 modding wiki 的离线参考文档（49 篇），技能内容的事实来源
- `AGENTS.md` — 面向 AI agent 的仓库说明（项目结构、编辑规范、资料来源），想了解项目内部组织也可以看

## 来源

技能内容整理自 [Hearts of Iron IV Wiki](https://hoi4.paradoxwikis.com/Hearts_of_Iron_4_Wiki) 的 modding 页面。
