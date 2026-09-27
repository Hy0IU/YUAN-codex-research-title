# YUAN-codex-research-title

让 Codex 科研对话的标题跟上持续研究目标，便于在 `My-Paper`、`MOE` 和实验结果项目中检索。

每轮对话结束后，Stop Hook 在后台读取最近几轮有效内容，由独立模型判断保留还是更新标题。标题采用 **`类别 emoji + 对象｜目标`**：对象优先使用具体论文、方法、模块、数据集、实验或研究问题；项目分组已提供的上下文不重复写入标题。

## 科研类别

| Emoji | 类别 | 常见任务 |
|---|---|---|
| 📚 | 文献与基线 | 检索、阅读、相关工作、基线可比性 |
| 💡 | 问题与创新 | idea、研究问题、假设、贡献边界 |
| 🧠 | 方法与模型 | 机制、架构、模块、公式 |
| 🧪 | 实验设计 | 消融、对照、指标、seed、实验协议 |
| 💻 | 代码与复现 | 实现、调试、训练、环境、复现 |
| 📊 | 结果与证据 | 指标分析、稳健性、表图一致性、主张核验 |
| ✍️ | 论文与审稿 | 结构、写作、润色、审稿和回复 |
| 🎨 | 图表与汇报 | 图表布局、方法图、演示文稿 |

示例：`📊 双轴结构消融｜核验 Fig. 3 与正文分析`、`💻 NYC t-SNE 绘图脚本｜保存论文复绘坐标`、`💡 Next-POI 超图推荐｜筛选研究问题`。分类边界和更多示例见 [科研分类](docs/科研分类.md)。

## 项目识别

插件将本地工作目录转换成不含绝对路径的弱项目提示，并区分 `MOE代码`、`MOE实验结果` 和 `MOE实验运行`。提示词把它当作消歧线索，不把目录名机械复制到标题。

## 安装

在 Codex 中请求安装 GitHub 仓库 `Hy0IU/YUAN-codex-research-title` 的**完整插件**（包含 Stop Hook，不要只复制 Skill），启用 Hooks，并在 Hook 管理界面信任该 Stop Hook。安装后先运行 `doctor`；再在新的本地对话中确认 Hook 实际触发。

需要 Python 3.10+、已登录且支持当前话题存储的本地 Codex CLI。Hook 只更新标题元数据，不往原对话追加消息。命名会使用当前 Codex 账号的模型额度。**云端和 ChatGPT Work 不支持本地 Hook。**

## 手动控制

插件安装后可让 Codex 预览标题、暂停或恢复自动命名、锁定标题和查看用量。脚本入口：

```sh
python3 scripts/yuan_codex_research_title.py doctor
python3 scripts/yuan_codex_research_title.py status
python3 scripts/yuan_codex_research_title.py usage
python3 scripts/yuan_codex_research_title.py rename <thread-id>
python3 scripts/yuan_codex_research_title.py rename <thread-id> --apply
python3 scripts/yuan_codex_research_title.py lock <thread-id>
python3 scripts/yuan_codex_research_title.py unlock <thread-id>
python3 scripts/yuan_codex_research_title.py pause
python3 scripts/yuan_codex_research_title.py resume
```

`rename` 默认只预览；只有带 `--apply` 才写标题。配置和状态默认位于 `$CODEX_HOME/yuan-codex-research-title`；可用 `YUAN_CODEX_RESEARCH_TITLE_DATA` 指定单独目录。

## 来源

本项目基于 [oil-oil/oil-codex-title](https://github.com/oil-oil/oil-codex-title)，保留其 MIT License 下的后台 Hook、官方 App Server 适配、状态保护和用量记录流程。此版本更换为 YUAN 的科研分类和项目提示，并不包含闲置话题归档功能。
