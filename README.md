# 🔮 Vedic Astrology · 吠陀占星完整分析系统

一个纯 Agent / Skill 形态的吠陀占星（Vedic / Jyotish）分析系统。它把完整的八模块占星工作流——从排盘、本命分析、出生时间校准，到职业、恋爱、合盘、卜卦——打包成一套可被 AI 助手直接加载的规则库与计算脚本，无需前端、无需后端服务。

> ✨ 本项目是一个 **Agent Skill**：复制到对应目录即可被 Codex / Claude Code 等 AI 助手识别并加载。没有 Web 界面，所有交互通过 AI 对话完成。

## 🔭 它能做什么

| 模块 | 入口 | 功能 |
|:-----|:-----|:-----|
| 🔭 **Reader** | `resources/reader.md` | 从 PDF、截图或文本提取并校验星盘数据 |
| 🪐 **Calculator** | `resources/calculator.md` | 从出生日期、时间和地点直接排盘，生成结构化星盘数据 |
| ⭐ **Core** | `resources/core.md` | P1–P12 本命盘、分盘、宫位、人生板块与问答 |
| 🕰️ **Rectifier** | `resources/rectifier.md` | 出生时间校准 |
| 🏛️ **Career** | `resources/career.md` | 职业与事业专题 |
| 💞 **Love** | `resources/love.md` | 恋爱、婚姻与时机专题 |
| ♾️ **Synastry** | `resources/synastry.md` | 双人关系 / 合盘 |
| 🃏 **Prashna** | `resources/prashna.md` | 独立提问盘 / 时盘（卜卦） |

## ✨ 特性

- 🕉️ **八大模块，各司其职**：本命、合盘与卜卦是三条相互独立的工作流，互不串用结论。
- 🪐 **真实天文计算**：基于 Swiss Ephemeris（pysweph）与 PyJHora 完成行星位置、分盘、大运、Ashtakavarga、Shadbala 等计算，而非查表。
- 📜 **完整证据链**：每个模块都有独立的数据契约与校验规则，判断只来自授权数据与规则，禁止用用户经历反推盘面。
- 🔮 **可扩展的卜卦栈**：Prashna 支持标准层、Tajika 叠加层与 KP 1–249 独立栈，三者产物与结论权限互相隔离。
- 🔒 **纯本地运行**：所有计算在本地完成，无需上传出生信息到任何服务。

## 🗂️ 目录结构

```
vedic-astrology/
├── SKILL.md              # Skill 入口，负责八模块路由
├── requirements.txt      # Python 依赖清单
├── resources/            # 38 个模块规则文件（Markdown）
│   ├── reader.md         #   读盘模块
│   ├── calculator.md     #   排盘模块
│   ├── core.md           #   本命核心分析
│   ├── rectifier.md      #   出生时间校准
│   ├── career.md         #   职业专题
│   ├── love.md           #   恋爱专题
│   ├── synastry.md       #   合盘
│   ├── prashna.md        #   卜卦
│   └── ...               #   各模块的细分规则
├── scripts/              # 22 个计算脚本（Python）
│   ├── engine.py         #   排盘核心引擎
│   ├── formatter.py      #   星盘数据格式化
│   ├── setup_env.py      #   环境一键安装
│   ├── ephe/             #   星历数据（.se1.txt）
│   └── ...               #   大运/分盘/合盘/卜卦等计算器
├── codex-patch/          # Codex 宿主兼容层
└── LICENSE               # AGPL-3.0
```

## 🌠 快速开始

### 📦 1. 安装为 Skill

**Codex**

```bash
git clone https://github.com/cindy-yuchen/vedic-astrology.git ~/.codex/skills/vedic-astrology
```

**Claude Code**

```bash
git clone https://github.com/cindy-yuchen/vedic-astrology.git ~/.claude/skills/vedic-astrology
```

安装后，在对话中提到「排盘」「看盘」「吠陀占星」「合盘」「起一卦」等即可触发。

### 🧭 2. 配置计算环境

排盘依赖 Swiss Ephemeris 等天文库，**请使用一键脚本安装**（它会正确处理包冲突与星历文件）：

```bash
cd ~/.codex/skills/vedic-astrology   # 或你的实际安装目录
python3 scripts/setup_env.py
```

> 不要直接 `pip install -r requirements.txt`。`dashaflow` 声明依赖已停更的 `pyswisseph`，而本项目实际使用社区 fork `pysweph`，直接安装会冲突。`setup_env.py` 通过 `--no-deps` 参数解决此问题。

### 📋 3. 环境要求

- **Python 3.8 ~ 3.13**（`pysweph` 为 C 扩展，暂无 3.14 预编译包）
- 首次配置时，`setup_env.py` 会识别 `scripts/ephe/*.se1.txt` 并复制为真实 `.se1` 星历文件；缺失的星历会从 astro.com 官方源自动补齐。

## 🧩 依赖

| 包 | 版本 | 用途 |
|:---|:-----|:-----|
| pysweph | ≥ 2.10.3.5 | Swiss Ephemeris 天文计算 |
| PyJHora | 4.8.6 | 分盘 / 大运 / Ashtakavarga / Shadbala |
| dashaflow | ≥ 0.3 | 大运（Dasha）计算 |
| pytz | ≥ 2024.1 | 时区处理 |

## ⚠️ 免责声明

本项目提供的是占星学计算与分析规则，内容仅供个人兴趣与参考，不构成医疗、法律、投资或任何人生重大决策的建议。

## 📜 License

[GNU Affero General Public License v3.0](LICENSE)（AGPL-3.0）——个人使用无限制；商用需开源全部服务端代码。
