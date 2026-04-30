# ascend-skills-eval

面向昇腾生态的 **SKILL.md 结构评测** 工具集：九维打分、改进建议、Markdown 报告与成果卡 PNG；可选部署 **FastAPI 在线服务**，在浏览器里粘贴文本、单仓库或批量仓库评测。

---

## 在线评测界面（Web UI）

![Ascend Skills-Eval web UI — paste / single repo / batch tabs, scores, local history](docs/screenshots/web-ui.png)

*左侧：标签切换（粘贴 SKILL / 单仓库 / 批量仓库）、可选仓库内路径、仓库链接与操作按钮。右侧：总分与维度、报告摘要、批量排行榜、本机最近 10 次历史（支持单条删除与「清空全部」）。*

---

## 功能要点

| 能力 | 说明 |
|------|------|
| 粘贴评测 | 直接粘贴完整 `SKILL.md`，一键评分并出图 |
| 单仓库 | 公开 HTTPS 仓库（GitHub / GitCode 等），服务端浅克隆后自动探测 `SKILL.md` |
| 批量仓库 | 多行 URL，返回排行榜、汇总 MD、可逐行切换成果卡 |
| 产物 | 维度分、文本建议、评分报告（MD）、成果卡（PNG，base64 或下载） |
| 本机历史 | 仅存 **浏览器 localStorage**（最近 10 条），可回放、单删、清空；**无服务端历史 API** |

---

## 仓库结构

```text
skills-eval/
├─ docs/screenshots/         # README 等文档用图
├─ skills/skills-eval/       # SKILL 工作流、评测模板、render-card / Playwright 脚本
├─ web-service/              # FastAPI 在线服务（Zeabur / 本地 uvicorn）
│  └─ app/static/index.html  # 内置评测页前端
└─ README.md                 # 本文件
```

---

## 本地开发

**前置条件**

- Python 3.10+（推荐）
- Node.js（用于 `render-card`：Playwright 调 Chromium 出 PNG）

**步骤**

```bash
cd web-service
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 首次需要 Chromium（成果卡渲染）
npm i -D playwright
npx playwright install chromium

uvicorn app.main:app --host 0.0.0.0 --port 8000
```

**访问**

| URL | 用途 |
|-----|------|
| [http://127.0.0.1:8000/](http://127.0.0.1:8000/) | 内置评测页 |
| [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health) | 健康检查 |

---

## HTTP API 速查

| 方法 | 路径 | 说明 |
|------|------|------|
| `POST` | `/eval` | 仅结构评估（JSON 入参：`skill_markdown` 等） |
| `POST` | `/evaluate-and-render` | 评估 + 渲染成果卡 PNG |
| `POST` | `/evaluate-repo` | 单仓库克隆 + 评估 + 出图 |
| `POST` | `/evaluate-repos` | 批量仓库（最多 20 个），含排行榜与汇总 MD |
| `POST` | `/render-card` | 仅根据 `card_data` JSON 渲染 PNG |
| `GET` | `/` | 返回内置 `index.html` |
| `GET` | `/health` | 存活探针 |

评测历史由前端写入 **localStorage**，服务端不持久化用户记录；亦无 `GET /history` 类接口。

更细的 curl 示例与 Zeabur 说明见 [**web-service/README.md**](web-service/README.md)。

---

## 部署（Zeabur）

- 使用 **`web-service/Dockerfile`** 构建，监听 **`8000`**，健康检查路径 **`/health`**。  
- 详细步骤与 API 示例以 [web-service/README.md](web-service/README.md) 为准。

---

## 致谢

本项目的核心思路与工作流，受到 [alchaincyf/darwin-skill](https://github.com/alchaincyf/darwin-skill) 的启发。  
感谢原项目在「评估 → 改进 → 测试 → 保留/回滚」循环上的优秀实践。

`ascend-skills-eval` 在此基础上面向昇腾场景增强：NPU/Ascend 相关 rubric、仓库拉取评测、批量排行与可视化产物等。

---

## License

MIT
