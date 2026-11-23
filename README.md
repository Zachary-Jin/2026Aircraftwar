# NewAircraftwar

将 Java/Swing 版《飞机大战》迁移到 Android。目标是交付竖屏触控射击、三档单机难度、本地与联网排行榜，以及双人实时分数对战。

*An Android migration of a Java/Swing aircraft shooter for a software construction course.*

[开发状态](#开发状态) · [Quick Start](#quick-start) · [版本对照](#版本对照) · [变更记录](#变更记录) · [English](#english)

## 开发状态

**项目已进入安卓迁移阶段。** 仓库中的桌面版用于复现原有玩法，安卓开发按以下顺序推进。

| 模块 | 当前进度 |
| --- | --- |
| 桌面对照基线 | 已入仓。包括三档难度、敌机与道具、音效和本地 CSV 排行榜。[固定基线 47c577c](https://github.com/Zachary-Jin/AircraftWar/tree/47c577c66776147cd3a47da75eadef87fc41e9b5) |
| Java 战斗内核 | 五种弹道已在独立的本地迁移工程中验证，接下来将纳入本仓库。随后迁移实体、碰撞、计分和道具。 |
| Android 客户端 | 计划接入固定步长内核、Canvas 绘制、触控和生命周期管理，先跑通一局单机。 |
| 存储与网络 | 计划用 Room 保存本地成绩。服务端提供 HTTPS 云榜和 WebSocket 双人比分对战。 |

## Quick Start

### 安卓内核原型

已有本地 `AndroidAircraftwar` 迁移工程的开发者，可以在该工程根目录运行下列命令。原型使用 Gradle Wrapper，已在 JDK 25 上验证。此阶段可在 JVM 上编译和测试弹道代码。

```bash
# macOS / Linux
./gradlew :core:test :core:jar
```

```powershell
# Windows PowerShell
.\gradlew.bat :core:test :core:jar
```

Android 客户端入仓后，将在这里同步 SDK、运行配置和 APK 构建命令。

### 运行桌面对照基线

需要 JDK 11 或更高版本、Maven 和桌面图形环境。从仓库根目录执行。

```bash
git clone https://github.com/Zachary-Jin/2026Aircraftwar.git
cd 2026Aircraftwar
mvn test
java -cp target/classes edu.hitsz.application.Main
```

桌面版用鼠标拖动英雄机并自动射击。资源按相对路径加载，请保持运行目录为仓库根目录。[原桌面工程](https://github.com/Zachary-Jin/AircraftWar)

## 版本对照

| 能力 | 桌面基线 | 安卓目标 |
| --- | --- | --- |
| 操作与绘制 | 鼠标、Swing 窗口 | 触控与竖屏适配，Canvas 按状态快照绘制 |
| 战斗循环 | 桌面定时器和面板 | 每局独立的 Java 内核，以 40 ms 固定步长更新 |
| 结算与存储 | 结算弹窗、按难度分开的 CSV | 单局生成一次结算结果，Room 保存成绩 |
| 联网能力 | 单机游戏和本地榜单 | 云榜按对局 ID 去重，双人使用共同刷怪脚本比拼分数 |

[比较桌面原始提交与当前 main](https://github.com/Zachary-Jin/2026Aircraftwar/compare/c11d558...main)

## 变更记录

### 2026-11-23 · 桌面基线与安卓迁移启动

- **桌面基线** 修复撞机漏判、单发重复命中、重复冰冻和 CSV 坏行中断读取。
- **安卓进度** 本地纯 Java 原型已完成不射击、直射、双发直射、散射和环射。5 项内核测试与 30 组新旧弹道对照通过。
- **仓库文档** 将开发状态、Quick Start 和版本对照集中到本页，在 `AGENTS.md` 中登记提交记录约定。仓库内 4 项桌面测试通过，已核对桌面入口与资源路径。

### 提交记录约定

每次提交都在本 README 的「变更记录」追加一条，最新记录放在最上方。写明日期、版本或开发阶段、实际改动及验证结果。涉及版本、模块状态、依赖或运行命令时，同一提交同步更新对应章节。验证范围按实际执行情况记录。

## English

This project ports a desktop aircraft shooter to Android. The migration starts with a standalone Java combat core, followed by touch controls, rendering, local scores, a cloud leaderboard, and two-player score challenges.

The repository currently contains the runnable desktop baseline. The Java prototype for all five firing patterns has been verified in a separate local project. See [Quick Start](#quick-start) for both entry points and [Change history](#变更记录) for the current milestone.
