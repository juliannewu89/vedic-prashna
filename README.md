<p align="center">
  <h1 align="center">🔮 vedic-prashna</h1>
  <p align="center">
    <strong>吠陀即时盘（Prashna / 问事盘）引擎 | Vedic Horary (Prashna) Engine for Claude Code</strong>
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/python-3.8~3.13-green" alt="Python">
    <img src="https://img.shields.io/badge/license-AGPL--3.0-orange" alt="License">
    <img src="https://img.shields.io/badge/platform-Claude%20Code-blue" alt="Platform">
  </p>
</p>

---

> 以提问时刻与提问地点起盘，回答一个具体问题的成败、展开方式与时间窗口。一张盘只回答一个问题。所有可验证的预测在结果出现之前写入预注册清单，准确率以长期验证统计为准。
>
> Prashna is Vedic horary astrology. A chart is cast for the exact time and place a question is asked, and each chart answers a single question: will it happen, how will it unfold, and when. Every verifiable prediction is recorded before the outcome is known, and accuracy is tracked against actual results over time.

本仓库包含两个 Claude Code skill。 This repository contains two Claude Code skills:

| Skill | 作用 / Role | 来源 / Origin |
|:---|:---|:---|
| 🔮 **vedic-prashna** | 即时盘判读引擎（规则、纪律、输出模板）/ Horary reading engine (rules, discipline, output templates) | 本仓库新增 / New in this repository |
| 🧮 **vedic-calculator** | 排盘计算引擎，角秒级精度 / Chart calculation engine, arc-second precision | [CNWU16/vedic-astro-skills](https://github.com/CNWU16/vedic-astro-skills) v7.0，原样保留 / unmodified |

vedic-calculator 出自 [CNWU16/vedic-astro-skills](https://github.com/CNWU16/vedic-astro-skills)（AGPL-3.0）。该项目另有本命盘审计、事业、感情、合盘、生时矫正等完整 skill 体系，需要本命盘分析的用户请移步上游仓库。感谢原作者的开源工作。

vedic-calculator comes from [CNWU16/vedic-astro-skills](https://github.com/CNWU16/vedic-astro-skills) (AGPL-3.0). The upstream project also provides a complete natal-analysis suite (life audit, career, love, synastry, birth-time rectification); users looking for natal analysis should visit the upstream repository. Thanks to the original author for the excellent open-source foundation.

**语言 / Language**: 中文与英文提问均可，回复与报告使用提问者的语言。 Works in both Chinese and English; replies and reports follow the language of the question.

---

## 📦 安装 / Installation

### Step 1: 安装 skill 文件 / Install the skill files

```bash
git clone https://github.com/juliannewu89/vedic-prashna.git
cp -r vedic-prashna/claude-code/skills/vedic-* ~/.claude/skills/
```

两个 skill 都必须安装：vedic-prashna 负责判读，vedic-calculator 负责排盘。缺少 calculator 时 prashna 无法起盘。

Both skills are required: vedic-prashna does the reading, vedic-calculator does the calculation. Without the calculator, prashna cannot cast a chart.

### Step 2: 安装 Python 依赖 / Install Python dependencies

| 要求 Requirement | 说明 Details |
|:---|:---|
| Python | 3.8 ~ 3.13（3.14 暂不支持 / not yet supported） |
| 安装方式 Install | 运行 `setup_env.py` / run `setup_env.py`（见下方 / see below） |

```bash
# 使用自动安装脚本（处理依赖冲突并自动验证）
# Use the auto-setup script (handles dependency conflicts and validates)
python vedic-calculator/scripts/setup_env.py

# 注意：不要直接 pip install -r requirements.txt（存在 dashaflow 依赖冲突）
# Note: do NOT pip install -r requirements.txt directly (dashaflow conflict)
```

AI 首次运行时会自动检测环境并执行 `setup_env.py`，但需确保系统已安装 Python 3.8~3.13。

The AI agent auto-detects the environment and runs `setup_env.py` on first use; make sure Python 3.8~3.13 is installed.

---

## ⚡ 使用 / Usage

对 Claude Code 说"起个盘"并描述一件具体的事，即可触发。 To use it, ask Claude Code about one specific thing:

```
用户: 起个盘，这次面试会拿到 offer 吗？
User:  Cast a prashna — will I get the offer from this interview?

 AI: → 以当前时刻 + 用户所在城市排盘（vedic-calculator）
    → 按规则判读：归宫 → 判成败 → 判动静 → 月亮剧本 → 时间窗口
    → 输出判词 + 预注册清单，写入 cases/<名字>/qa_prashna_<主题>.md
    → 在 prashna_log.md 追加一条记录，待结果出现后回填验证

 AI: → casts a chart for the current moment + the user's city (vedic-calculator)
    → reads by rule: house assignment → outcome → mobility → Moon sequence → timing
    → delivers a verdict + pre-registered predictions, saved to cases/<name>/qa_prashna_<topic>.md
    → appends one line to prashna_log.md, updated once the outcome is known
```

**触发词 / Triggers**: 起个盘 · 即时盘 · 问事盘 · 这事能成吗 · cast a prashna · horary chart · will this work out

**适用 / In scope**: 一件具体的、可验证的事（面试、签约、搬家、租房、出行、找回失物等）。 A specific, verifiable question: an interview, a contract, a move, a rental, a trip, a lost item.

**不适用 / Out of scope**: 人生方向、婚姻大势等长线问题，此类问题属于本命盘体系（见上游仓库）。 Big-picture questions (life direction, long-term relationship prospects) belong to natal analysis; see the upstream repository.

---

## 📐 判读体系 / Method

| 步骤 / Step | 内容 / What happens |
|:---|:---|
| 归宫 House assignment | 按问题类型确定事项宫（Karya Bhava）/ map the question to its house, per `resources/karya_bhava.md` |
| 判成败 Outcome | 命主星/月亮与事主星之间的连接 / connection between Lagna lord / Moon and the karya lord |
| 判动静 Mobility | 上升星座性质（活动/固定/双元）/ movable, fixed, or dual ascendant |
| 读展开 Development | 月亮接下来的入相位序列 / the Moon's upcoming applying aspects as the step-by-step script |
| 读时间 Timing | 引擎逐日扫描真实天象（逆行转顺、精确合相、换座）/ day-by-day scan for real events: stations, exact aspects, sign changes |
| 输出 Output | 判词 + 预注册清单 + 边界声明 / verdict + pre-registered predictions + scope statement |

### 执行纪律 / Discipline

- **预注册 / Pre-registration**: 所有可验证的预测在结果出现之前写入清单，事后仅作对照，不修改原文。 Predictions are written down before the outcome is known. After that they can be checked against reality, but never edited.
- **明示不确定性 / Stated uncertainty**: 盘面信号不足时如实说明，不强行给出判断。 When a chart is weak or contradictory, the reading says so rather than forcing a verdict.
- **长期验证 / Long-term verification**: 每张盘的命中情况记录于 `prashna_log.md`，准确率以长期统计为准。 Every hit and miss is logged in `prashna_log.md`; accuracy is a long-run statistic, not a claim.
- **一问一盘 / One chart per question**: 同一问题不重复起盘；管辖外的问题另起新盘。 The same question is never re-cast, and unrelated questions get charts of their own.

排盘由 vedic-calculator 完成，判读严格依照 `resources/` 下的规则文件执行，不允许凭记忆估算行星位置。

Calculation is handled by vedic-calculator; interpretation strictly follows the rule files under `resources/`, and planetary positions are never estimated from memory.

---

## 📁 项目结构 / Project Structure

```
vedic-prashna/
├── README.md
├── LICENSE                          # AGPL-3.0
├── COMMERCIAL_NOTICE                # 指令文件商用限制 / commercial-use restriction (inherited from upstream)
└── claude-code/skills/
    ├── vedic-prashna/               # 即时盘判读引擎 / horary reading engine (new in this repo)
    │   ├── SKILL.md                 # 起盘流程与判读步骤 / casting flow and reading steps
    │   └── resources/
    │       ├── discipline.md        # 纪律与留档规范 / discipline and record-keeping (highest priority)
    │       ├── karya_bhava.md       # 归宫表 / house-assignment table
    │       ├── reading_rules.md     # 判成败/动静/月亮剧本/时间/竞争 / outcome, mobility, Moon script, timing, competition
    │       └── sign_descriptors.md  # 星座词表（标注象征层）/ sign descriptors (symbolic layer, flagged)
    └── vedic-calculator/            # 排盘引擎，出自上游，原样保留 / calculation engine from upstream, unmodified
        ├── SKILL.md
        ├── requirements.txt
        └── scripts/                 # engine.py + PyJHora 封装 + 星历数据 / engine.py + PyJHora wrappers + ephemeris data
```

---

## 🧪 技术 / Technical Notes

| 项目 Item | 说明 Details |
|:---|:---|
| 流派 School | Parashari（KN Rao 体系）；prashna 判读规则为本仓库整理 / Parashari (KN Rao); horary rules compiled in this repository |
| Ayanamsa | True Chitra |
| 天文核心 Ephemeris | Swiss Ephemeris（pysweph），行星位置精度 < 0.01° / planetary positions accurate to < 0.01° |
| 排盘引擎 Engine | PyJHora 4.8.6 封装，含上游的 9 项 Shadbala 修正 / PyJHora 4.8.6 wrappers with upstream's 9 Shadbala fixes |
| 跨平台 Cross-platform | Windows / macOS / Linux，Python 3.8~3.13 |

引擎精度验证数据见上游仓库。 Engine accuracy benchmarks are documented in the upstream repository: [CNWU16/vedic-astro-skills](https://github.com/CNWU16/vedic-astro-skills).

---

## License

**代码 / Code**: [AGPL-3.0](LICENSE)。可自由使用、修改、学习；基于本项目提供网络服务（SaaS）需开源完整服务端代码。 Free to use, modify, and study; network services built on it must open-source their full server-side code.

**指令文件 / Prompt files**（SKILL.md / resources）: 附加商用限制，个人使用无限制，商业使用需取得授权，详见 [COMMERCIAL_NOTICE](COMMERCIAL_NOTICE)。 Personal use is unrestricted; commercial use requires permission, see [COMMERCIAL_NOTICE](COMMERCIAL_NOTICE). vedic-calculator 的商用授权请联系上游原作者。 For commercial licensing of vedic-calculator, contact the upstream author: [CNWU16/vedic-astro-skills](https://github.com/CNWU16/vedic-astro-skills).
