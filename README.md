# Slay the Spire Bot Mod

一个基于 **ModTheSpire + BaseMod** 的 `Slay the Spire` 自动爬塔模组原型，当前以 `Ironclad` 为唯一目标角色。

这个项目的重点不是把整套流程“跑起来”而已，而是在流程稳定的前提下，持续提高 bot 的实际决策质量，包括：

- 战斗出牌顺序
- 战斗中药水使用
- 战斗中遗物联动
- 地图与路径选择
- 奖励、商店、篝火、事件等非战斗决策

## 当前定位

当前 bot 已经具备一条稳定的主流程：

- `capture -> decide -> execute -> validate/log`

已覆盖的主要界面：

- 战斗
- 地图
- 卡牌奖励
- 战后奖励
- 事件
- 商店
- 篝火
- Boss relic 三选一

交互热键：

- `F8`：启动 / 暂停 bot
- `F9`：单步执行一次决策

推荐优先使用 `F9` 做单步验证，再逐步切到连续运行。

## 当前已完成的决策优化

### 战斗

已支持基础战斗启发式与两回合浅层序列搜索，重点提升“本回合该先打什么”的质量。

近期已补强的战斗决策包括：

- 多类战斗药水：
  - `Colorless Potion`
  - `Attack Potion`
  - `Skill Potion`
  - `Power Potion`
  - `Fear Potion`
  - `Ancient Potion`
  - `Cultist Potion`
  - `Speed Potion`
  - `Duplication Potion`
- 已实现的战斗遗物联动：
  - `Anchor`
  - `Horn Cleat`
  - `Champion Belt`
  - `Paper Frog`
  - `Ornamental Fan`
  - `Kunai`
  - `Shuriken`
  - `Pen Nib`
  - `Nunchaku`
  - `Letter Opener`
  - `Ink Bottle`
  - `Gremlin Horn`
  - `Akabeko`

这些优化已经不再只是“识别到遗物”，而是会直接影响：

- 出牌顺序
- 进攻与防守的取舍
- 补刀与续航节奏
- setup 牌是否值得先打

### 地图与路径

地图决策已经从简单的“看当前节点类型”提升到包含未来路径形状的启发式评估，当前会综合考虑：

- 当前血量与生存风险
- potions 与牌组质量
- elite 意愿
- shop / purge / upgrade 的潜在收益
- 接近 boss 时的路线偏置

已加入的路径级逻辑包括：

- 连续 elite 惩罚
- elite 后接 `REST` / `SHOP` 的奖励
- 接近 boss 的危险链惩罚
- `Slime Boss`、`Guardian` 等 boss-aware 路线偏置

### 奖励、商店、事件

- 卡牌奖励已支持部分 boss-aware 选择偏置
- 商店已支持 `relic / card / purge` 的基础权衡
- 战后奖励已支持 relic、potion、gold、钥匙等基础排序
- 特殊事件支持：
  - `GremlinWheelGame`
  - `GremlinMatchGame`
- `Neow` 事件已加入定向规则，避免选择“前 3 场战斗敌人只有 1 HP”的低价值开局选项

## 当前验证方式

常用构建与回归命令：

```powershell
.\gradlew.bat offlineTest
.\gradlew.bat check
.\gradlew.bat jar
```

当前构建产物：

- `build/libs/slay-the-spire-bot-0.1.0.jar`

离线测试主要覆盖：

- 决策引擎启发式
- 战斗 relic / potion 联动
- 地图路径评估
- 特殊事件支持
- 执行动作与状态适配

## 安装

1. 构建输出 jar
2. 将 jar 放入 `Slay the Spire/Mods/`
3. 用 `ModTheSpire` 启动游戏并勾选本 Mod

依赖 jar 请放入 `lib/`，通常至少包括：

- `desktop-1.0.jar`
- `ModTheSpire.jar`
- `BaseMod.jar`

如果环境变量 `STS_MODS_DIR` 已指向游戏 `Mods` 目录，也可以执行：

```powershell
.\gradlew.bat copyToMods
```

当前仓库默认测试部署目录为：

- `D:\SteamLibrary\steamapps\workshop\content\646570\3701751907`

## 调试与日志

- 日志默认写入游戏工作目录下的 `logs/sts-bot/`
- 推荐优先使用 `F9` 单步调试，而不是直接长时间 `F8`
- 排查卡住问题时重点查看：
  - `ACTION`
  - `WARN`
  - `ERROR`

建议重点关注这些字段：

- `result=...`
- `post=...`
- `State did not change after action`
- `Bot auto-paused after repeated identical state/action loops`

## 更高效的本地测试

推荐直接使用一键测试脚本：

```powershell
.\scripts\dev-test.ps1
```

它会自动：

- 构建最新 jar
- 部署到当前实际 mod 安装目录
- 清理该目录下本 mod 的旧 jar，避免误测旧版本

常用参数：

```powershell
.\scripts\dev-test.ps1 -WhatIf
.\scripts\dev-test.ps1 -ShowLatestLog -LogDir '你的logs/sts-bot目录'
.\scripts\dev-test.ps1 -LaunchGame -GameExecutable '你的游戏或启动器路径'
```

说明：

- `STS_MODS_DIR` 可覆盖默认部署目录
- `STS_GAME_EXE` 可提供默认游戏启动路径
- `STS_BOT_LOG_DIR` 可提供默认日志目录

## 下一步优化计划

下一阶段仍然以“增量提高胜率”为目标，不做大重写，继续沿用：

- 小步迭代
- 离线测试先行
- 实机回归补验证

### 1. 继续补强战斗遗物联动

优先继续扩展会明显改变出牌顺序的 relic，重点是：

- 能改变本回合生存线的 relic
- 能改变补刀与续航节奏的 relic
- 能改变 setup 与爆发顺序的 relic

实现原则：

- 只增加必要的快照状态字段
- 不引入泛化的大型 relic 状态系统
- 每加一类 relic，都同步补离线测试

### 2. 提升篝火与长期价值判断

当前篝火决策还比较粗，下一步重点补：

- `REST` vs `SMITH` 的更细粒度权衡
- boss、当前血量、牌组成长性之间的联动
- 对高价值升级窗口的识别

### 3. 继续细化奖励与商店决策

重点补强：

- boss-aware 卡牌奖励的继续微调
- 商店中 `relic / purge / card` 的更精细排序
- 大牌组、中后期低质量买牌倾向的进一步压制

### 4. 扩展事件支持

继续把“不适合走通用按钮逻辑”的事件逐步纳入 `SpecialEventSupport`，减少复杂事件中的误判和卡住。

### 5. 做更多实机回归

在离线测试通过的前提下，继续按 `docs/regression-checklist.md` 做实机回归，重点关注：

- 地图切换
- 事件
- 商店
- 奖励
- 篝火
- 跨层与跨局状态

## 项目状态

这个项目目前仍然是一个持续迭代中的 bot 模组，不是“通关级成品 AI”。  
当前阶段最重要的工作不是扩大功能面，而是把已有流程上的每一个关键决策逐步做对。
