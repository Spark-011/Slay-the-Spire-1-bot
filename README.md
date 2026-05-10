# Slay the Spire Bot Mod

一个基于 **ModTheSpire + BaseMod** 的《Slay the Spire》自动爬塔模组，以铁甲战士（Ironclad）为目标角色。

这个项目的重心不是让 bot "跑得通"——流程稳定性已经是前提。真正持续在做的事，是在流程稳定的前提下，不断提升 bot 的实际决策质量，包括：

- 战斗出牌顺序
- 战斗中药水使用
- 战斗中遗物联动
- 地图与路径选择
- 奖励、商店、篝火、事件等非战斗决策

## 功能概览

bot 已经具备一条稳定的主流程：

`capture → decide → execute → validate/log`

已覆盖的主要界面：

- 战斗
- 地图
- 卡牌奖励
- 战后奖励
- 事件
- 商店
- 篝火
- Boss relic 三选一

### 战斗决策

已支持基础战斗启发式与两回合浅层序列搜索，重点提升"本回合该先打什么"的质量。

已实现的战斗药水支持：

`Colorless Potion` · `Attack Potion` · `Skill Potion` · `Power Potion` · `Fear Potion` ·
`Ancient Potion` · `Cultist Potion` · `Speed Potion` · `Duplication Potion`

已实现的战斗遗物联动：

`Anchor` · `Horn Cleat` · `Champion Belt` · `Paper Frog` · `Ornamental Fan` ·
`Kunai` · `Shuriken` · `Pen Nib` · `Nunchaku` · `Letter Opener` · `Ink Bottle` ·
`Gremlin Horn` · `Akabeko`

这些优化不只是"识别到遗物"，而是会直接影响出牌顺序、攻守取舍、补刀与续航节奏、以及 setup 牌是否值得先打。

### 地图与路径

地图决策综合考虑：

- 当前血量与生存风险
- 药水与牌组质量
- 精英怪意愿
- 商店/删牌/升级的潜在收益
- 接近 Boss 时的路线偏置

路径级逻辑包括连续精英惩罚、精英后接休息/商店的奖励、接近 Boss 的危险链惩罚，以及针对史莱姆 Boss、守护者等特定 Boss 的路线偏置。

### 奖励、商店、事件

- 卡牌奖励支持部分 Boss-aware 选择偏置
- 商店支持遗物/卡牌/删牌的基础权衡
- 战后奖励支持遗物、药水、金币、钥匙等基础排序
- 特殊事件支持：转盘事件（GremlinWheelGame）、配对事件（GremlinMatchGame）
- Neow 事件已加入定向规则，避免低价值开局选项

## 安装

### 方式一：从 Release 获取（推荐）

1. 从本仓库的 [Releases] 页面下载最新版本的 `slay-the-spire-bot-<版本号>.jar`
2. 将 jar 放入 `Slay the Spire/Mods/` 目录
3. 用 ModTheSpire 启动游戏，在模组列表中勾选本模组

### 方式二：从 Steam 创意工坊获取

如果本模组已上架 Steam 创意工坊，直接在《Slay the Spire》的创意工坊中搜索订阅即可。ModTheSpire 和 BaseMod 也会自动作为依赖安装。

### 前置依赖

- ModTheSpire（模组加载器）
- BaseMod（基础模组框架）

首次使用 ModTheSpire 请确保以上两个依赖也已就位。

## 交互热键

- **F8**：启动 / 暂停 bot
- **F9**：单步执行一次决策

推荐优先使用 **F9** 做单步观察，确认决策符合预期后再切换为连续运行。

## 日志与排查

- 日志默认写入游戏工作目录下的 `logs/sts-bot/`
- 如果 bot 出现卡住，推荐用 F9 单步执行来观察，比长时间 F8 连续运行更便于定位问题
- 排查卡住时，日志中重点关注：
  - `ACTION`
  - `WARN`
  - `ERROR`
  - `State did not change after action` 等异常标记

## 项目状态

这个项目目前仍然是一个持续迭代中的 bot 模组，不是"通关级成品 AI"。当前阶段最重要的工作不是扩大功能面，而是把已有流程上的每一个关键决策逐步做对。
