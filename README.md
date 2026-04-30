# ascend-skills-eval

嗨，欢迎路过 👋  

这是一套面向 **昇腾生态** 的 `SKILL.md` **结构评测**小工具：九维打分、改进建议、Markdown 报告，还有一张好看的 **成果卡 PNG** ✨  
想省事的话，可以跑起自带的 **FastAPI 网页**：在浏览器里粘贴全文、拉一个公开仓库，或者批量比一比排行榜——怎么顺手怎么来。

---

## 🖼️ 在线评测长什么样？

![Ascend Skills-Eval web UI — paste / single repo / batch tabs, scores, local history](docs/screenshots/web-ui.png)

*左边是「粘贴 / 单仓库 / 批量」三个标签，配上仓库路径和链接；右边是分数、维度、报告摘要，还有批量排行。最下面「本机最近 10 次」都存在你的浏览器里，支持回放、单条删除和一键清空——我们不会替你在服务器上存历史哦 🔒*

---

## ✨ 能帮你做什么？

| 能力 | 说明 |
|------|------|
| 📋 粘贴评测 | 把整份 `SKILL.md` 贴进去，点一下就有分数和图 |
| 🔗 单仓库 | 填 GitHub / GitCode 等 **HTTPS 公开仓库**，自动浅克隆并找 `SKILL.md` |
| 📊 批量仓库 | 多行 URL，一次出排行榜、汇总 MD，还能点每一行换成果卡 |
| 🎁 产出物 | 维度分、文字建议、报告 Markdown、成果卡 PNG（可下载） |
| 💾 本机历史 | 最近 10 条存在 **localStorage**，可回放 / 单删 / 清空全部；**没有**服务端历史接口 |

---

## 📦 仓库里大致有什么？

```text
skills-eval/
├─ docs/screenshots/         # 文档配图（比如上面的界面截图）
├─ skills/skills-eval/       # SKILL 工作流、评测模板、render-card / Playwright 脚本
├─ web-service/              # FastAPI 服务（本地或你自己的部署）
│  └─ app/static/index.html  # 内置评测页前端
└─ README.md                 # 你正在看的这份说明
```

---

## 🚀 本地跑起来玩玩

**需要准备**

- Python **3.10+**（再旧没细测，遇到问题欢迎提 Issue）
- **Node.js**（用来跑 Playwright，给成果卡截图用）

**复制粘贴即可**

```bash
cd web-service
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 第一次需要装个 Chromium（渲染 PNG 用）
npm i -D playwright
npx playwright install chromium

uvicorn app.main:app --host 0.0.0.0 --port 8000
```

**打开浏览器**

| URL | 做什么 |
|-----|--------|
| [http://127.0.0.1:8000/](http://127.0.0.1:8000/) | 评测主页 |
| [http://127.0.0.1:8000/health](http://127.0.0.1:8000/health) | 看看服务活没活 |

更多命令行示例、接口细节，可以翻翻 [**web-service/README.md**](web-service/README.md)——那边写得更「工程师向」一点 📎

---

## 📝 HTTP API 一览（给对接用）

| 方法 | 路径 | 一句话 |
|------|------|--------|
| `POST` | `/eval` | 只打分，不出图 |
| `POST` | `/evaluate-and-render` | 打分 + 成果卡 PNG |
| `POST` | `/evaluate-repo` | 克隆单仓库 + 打分 + 出图 |
| `POST` | `/evaluate-repos` | 批量仓库（≤20），含排行榜和汇总 MD |
| `POST` | `/render-card` | 手里已有 JSON，只渲染图 |
| `GET` | `/` | 返回内置网页 |
| `GET` | `/health` | 健康检查 |

历史记录都在你自己浏览器里，**服务端不存、也没有** `GET /history` 这类接口——放心折腾，换台电脑就是全新开始 😄

---

## 🤝 欢迎贡献

我们很乐意收到 **PR** 和 **Issue**——无论是修错别字、补文档，还是改评测逻辑，都很珍贵。

大致流程可以这样：

1. **Fork** 本仓库，从 `main` 拉一个功能分支（名字随意，能看懂最好）。
2. 尽量 **小步提交**：一个 PR 解决一类问题，方便 review。
3. 改完在 PR 里简单说说 **动机** 和 **怎么测**；如果动到了前端或 API，能贴截图或 curl 就更棒啦。
4. 保持 **友善沟通**；有分歧很正常，对事不对人 🙌

也欢迎到 [**Issues**](https://github.com/huxiaoman7/ascend-skills-eval/issues) 里提想法、报 bug、或者单纯问一句「这样用对不对」——我们都会看。

---

## 🔐 安全说明

若你发现 **安全漏洞**，请尽量不要在公开 Issue 里贴复现细节；可以发 **私有安全报告**（GitHub Security Advisories）或先开 Issue 仅标题示意，我们再私下对接。感谢帮忙让项目更安全 🛡️

---

## 🙏 致谢

思路与「评估 → 改进 → 测试 → 保留/回滚」这一套，深受 [**darwin-skill**](https://github.com/alchaincyf/darwin-skill) 启发，特此鸣谢。  

`ascend-skills-eval` 在此基础上做了昇腾向的扩展：NPU / Ascend 相关 rubric、仓库评测、批量排行和可视化小卡片等等。站在巨人的肩膀上，我们继续往前挪一小步 💪

---

## License

MIT — 拿去用、拿去改，记得保留许可证声明就好 📄
