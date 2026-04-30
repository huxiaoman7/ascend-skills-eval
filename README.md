# ascend-skills-eval

面向昇腾生态的 Skills 评测与进化工具集。  
在通用 Skill 优化方法的基础上，补齐了 Ascend/NPU 场景下的评估、报告与可视化能力。

---

## 致谢

本项目的核心思路与工作流，受到 [alchaincyf/darwin-skill](https://github.com/alchaincyf/darwin-skill) 的启发。  
感谢原项目在「评估 → 改进 → 测试 → 保留/回滚」循环上的优秀实践。

`ascend-skills-eval` 是在此基础上面向昇腾场景做的增强版，重点增加了：

- 昇腾/NPU 关键词与流程的评测适配
- 评分报告与评分卡（PNG）自动生成
- 仓库链接评测（GitHub/GitCode）与批量排行榜
- 在线页面；评测历史仅在浏览器本地保存（localStorage）与回放

---

## 项目结构

```text
skills-eval/
├─ skills/
│  ├─ skills-eval/           # SKILL.md、评测模板、截图与渲染脚本
│  └─ ascend-darwin-skill/   # 兼容目录（历史/迁移保留）
└─ web-service/              # FastAPI 在线服务（Zeabur 可部署）
```

---

## 核心能力

- 文本评测：直接粘贴 `SKILL.md` 内容评分
- 仓库评测：输入 GitHub/GitCode 链接自动拉取评分
- 批量评测：多个仓库一键评分并生成排行榜
- 可视化产物：自动生成评分报告（Markdown）与评分图（PNG）
- 历史回放：Web 页保留本机最近 10 次（localStorage），可点击回放

---

## 快速开始（本地）

```bash
cd web-service
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 首次需要浏览器依赖（评分卡渲染）
npm i -D playwright
npx playwright install chromium

uvicorn app.main:app --host 0.0.0.0 --port 8000
```

访问：

- `http://127.0.0.1:8000/` 在线页面
- `http://127.0.0.1:8000/health` 健康检查

---

## 主要 API

- `POST /eval`：文本评测
- `POST /evaluate-and-render`：文本评测 + 评分卡
- `POST /evaluate-repo`：单仓库链接评测
- `POST /evaluate-repos`：批量仓库链接评测

（评测历史由前端写入浏览器 localStorage，无独立历史 API。）

---

## 部署（Zeabur）

- Service 类型：Dockerfile
- Dockerfile 路径：`web-service/Dockerfile`
- 端口：`8000`
- 健康检查：`/health`

---

## License

MIT
