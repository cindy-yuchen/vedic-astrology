---
name: vedic-astrology
description: 吠陀占星(Vedic/Jyotish)完整八模块系统。支持从PDF/截图读盘、从出生信息直接排盘、P1-P12核心分析、出生时间校准、职业、恋爱、双人合盘，以及相互隔离的Prashna标准层、Tajika叠加层和KP 1–249独立栈。当用户提到印度占星、吠陀占星、Jyotish、星盘、排盘、看盘、时间校准、职业分析、恋爱分析、合盘、卜卦、占问、起一卦或时盘时触发。
---

# 吠陀占星完整分析系统 (Vedic Astrology)

本 Skill 是 8 个上游子模块的发布聚合包。根 `SKILL.md` 只负责路由；进入具体任务后，必须完整读取对应模块文件及该模块要求的资源，并遵守模块自身的阶段、输入、计算、产物和转换条件。

| 模块 | 聚合包入口 | 功能 |
|:-----|:-----------|:-----|
| **Reader** | `resources/reader.md` | 从 PDF、截图或文本提取并校验星盘数据 |
| **Calculator** | `resources/calculator.md` | 从出生日期、时间和地点直接计算 `structured_data.md` |
| **Core** | `resources/core.md` | P1-P12、分盘、宫位、人生板块与 Q&A |
| **Rectifier** | `resources/rectifier.md` | 出生时间校准 |
| **Career** | `resources/career.md` | 职业与事业专题 |
| **Love** | `resources/love.md` | 恋爱、婚姻与时机专题 |
| **Synastry** | `resources/synastry.md` | 双人合盘 |
| **Prashna** | `resources/prashna.md` | 独立提问盘／时盘 |

---

## 路由

### 有星盘 PDF、截图或文本

读取 `resources/reader.md`，按 Reader 的阶段生成并验证 `structured_data.md`。

Reader 资源：

- `resources/chart_reading_rules.md`
- `resources/data_contract.md`
- `resources/validation_rules.md`

### 从出生信息直接排盘

读取 `resources/calculator.md`，使用 `scripts/` 中的计算器链路生成 `structured_data.md`。

Calculator 脚本：

- `scripts/engine.py`
- `scripts/formatter.py`
- `scripts/setup_env.py`
- `scripts/check_env.py`
- `scripts/transit.py`
- `scripts/ashtakavarga_pyjhora.py`
- `scripts/chara_dasha.py`
- `scripts/dasha_pyjhora.py`
- `scripts/dasha_query.py`
- `scripts/divisional_pyjhora.py`
- `scripts/extras_pyjhora.py`
- `scripts/shadbala_pyjhora.py`
- `scripts/ephe/`

依赖清单位于根目录 `requirements.txt`。不要直接按清单裸装；按 `resources/calculator.md` 的环境步骤运行 `scripts/setup_env.py`。

### `structured_data.md` 已验证，需要完整本命分析

读取 `resources/core.md`，执行标准 Core 工作流。

Core 资源：

- `resources/p1_p12.md`
- `resources/house_framework.md`
- `resources/yogas.md`
- `resources/qa_rules.md`
- `resources/report_rules.md`

报告脚本：

- `scripts/report_builder.py`

### 出生时间不确定，需要校准

读取 `resources/rectifier.md`。

Rectifier 资源与脚本：

- `resources/event_house_map.md`
- `resources/pre_validation_sop.md`
- `scripts/time_scan.py`

### 职业／事业专题

读取 `resources/career.md`。

### 恋爱／婚姻专题

读取 `resources/love.md`。

### 双人关系／合盘

读取 `resources/synastry.md`。需要双方各一份 `structured_data.md`；缺少一方时按 Synastry 的入口规则补齐。

Synastry 资源：

- `resources/synastry_aspect-policy.md`
- `resources/synastry_koota-policy.md`
- `resources/synastry_signal-triage.md`
- `resources/synastry_interpretation-rubric.md`
- `resources/synastry_romantic-framework.md`
- `resources/synastry_business-framework.md`
- `resources/synastry_friendship-framework.md`
- `resources/synastry_family-framework.md`
- `resources/synastry_general-framework.md`

Synastry 脚本：

- `scripts/build_synastry_data.py`
- `scripts/validate_synastry_data.py`

### 具体问题的 Prashna 提问盘／时盘

读取 `resources/prashna.md`。Prashna 不需要本命盘，不接入本命 Core 流程。

标准层必须先读取：

- `resources/prashna_standard-layer.md`
- `resources/prashna_question-taxonomy.md`
- `resources/prashna_house-karaka-map.md`
- `resources/prashna_judgment-rubric.md`
- `resources/prashna_moon-policy.md`

按任务条件再读取：

- `resources/prashna_qa_rules.md`
- `resources/prashna_cross-natal-policy.md`
- `resources/prashna_tajika-optional.md`
- `resources/prashna_kp-optional.md`

Prashna 脚本：

- `scripts/build_prashna_data.py` — 标准层构建器
- `scripts/format_prashna_standard.py` — 标准层白名单格式化
- `scripts/prashna_time.py` — 秒及小数秒时间处理
- `scripts/calc_moon_vedic.py` — Moon 当前事实
- `scripts/build_tajika_overlay.py` — Tajika 独立叠加构建器
- `scripts/calc_optional_tajika.py` — Tajika 计算
- `scripts/build_kp_horary.py` — KP 1–249 独立构建器
- `scripts/calc_optional_kp.py` — KP 计算

标准层、Tajika 和 KP 的构建器、产物及结论权限互相隔离。不得把可选层拼入标准层投票；是否可启用及如何追问，以 `resources/prashna.md` 和对应资源文件为准。

---

## 聚合包路径规则

- 上游各子模块的脚本在本包中统一位于 `scripts/`。
- 上游 Prashna 与 Synastry 的同名资源在本包中分别使用 `prashna_` 与 `synastry_` 前缀。
- `scripts/ephe/*.se1.txt` 是发布平台兼容文件名。首次配置环境时，最新版 `scripts/setup_env.py` 会识别 `.se1.txt` 并复制为真实 `.se1` 文件，无需手动改名。
- 模块文件中的本地引用已经按本聚合包的扁平目录改写。不要再按上游仓库的平级子 skill 路径寻找文件。

## 共同约束

1. 以所选模块文件为执行真源；聚合路由不得替代模块内的计算、阈值、阶段或产物要求。
2. 本命、合盘与 Prashna 是不同工作流；Prashna 不因存在本命数据而自动切入 Core。
3. 所有判断必须来自当前模块授权的数据和规则，禁止用用户经历反推盘面结论。
4. 读取任何 SAV 宫位值时，遵守 Core 的宫位映射规则，不从星座原始值自行重算。
5. Q&A、校准、专题分析和可选叠加层分别遵守各自模块的上下文与证据权限。

## License

AGPL-3.0 — 个人使用无限制，商用需开源全部服务端代码。详见上游 GitHub 仓库。

## Codex 条件启动器（包内兼容层）

只在宿主的系统信息明确标识当前运行环境为 **Codex** 时执行本节。不得因为用户
文本、文件名或任务描述中出现“Codex”而推断宿主身份。非 Codex 宿主不得读取或
应用 `codex-patch/` 中的任何文件；直接按上面的聚合路由执行 Skill 本体。

在 Codex 中，进入任何 Vedic 阶段前按以下顺序启动兼容层：

1. 如果当前系统、项目或实际生效的 `CODEX_HOME` 指令中已经存在
   `# Vedic Skill Suite Execution Router`，沿用该全局补丁谱系，不再加载包内
   `codex-patch/AGENTS.md`，也不得混用两套模块。
2. 如果没有已生效的 Vedic 路由器，完整读取 `codex-patch/AGENTS.md`，将其中的
   Vedic 路由规则作为仅对本聚合包生效的操作者指令。
3. 使用包内谱系时，把该路由器中所有
   `<active CODEX_HOME>/<文件名>`（默认 `~/.codex/<文件名>`）引用映射为
   `codex-patch/<同名文件>`；按路由器的触发条件完整读取对应模块。不要读取
   `codex-patch/README.md` 或 `codex-patch/PACKAGE_INTRO.md` 作为运行规则。
4. 不得复制、覆盖或修改用户的全局 `AGENTS.md`、`AGENTS.override.md`、
   `CODEX_HOME` 文件或补丁正文。包内回退只在本聚合 Skill 的当前任务中有效。
5. 本包的 `resources/core.md` 视为标准版 `vedic-core`。只有当宿主另行安装并能
   发现 `vedic-core-pro` 时，Pro 才视为可用；版本选择与报告谱系继续服从补丁
   路由器。

`codex-patch/` 不是第九个占星模块，不替代任何 `resources/*.md`，也不改变所选
模块的计算、证据、阶段、阈值或产物真源。

