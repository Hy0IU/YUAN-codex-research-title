---
name: yuan-codex-research-title
description: 管理本地 Codex 科研对话标题，检查自动命名 Hook，预览或应用改名、暂停/恢复自动命名、锁定/解锁标题及查看用量。用于用户明确管理本插件或 Codex 对话标题时；论文标题和普通科研写作任务不触发本 Skill。
metadata:
  compatibility: 需要本地 Codex 桌面环境、Python 3.10+、已登录且支持当前话题存储的 Codex CLI；云端和 ChatGPT Work 不支持本地 Hook。
---

# YUAN Codex Research Title

自动命名由独立 Stop Hook 执行。本 Skill 只处理用户明确要求的检查、预览和控制，不在普通对话结束时主动改名。

## 定位入口

执行本 Skill 的 `scripts/run.py`，它会定位插件中的后台程序。将下文 `<入口>` 替换为该脚本的绝对路径。

## 检查和配置

1. 运行 `python3 <入口> doctor` 检查 Python、Codex 路径和 App Server；Windows 使用 `py -3`。
2. 配置模型时使用 `configure --model <模型 ID> --service-tier fast` 或 `--service-tier standard`。模型和档位必须来自用户选择或当前可用列表，不猜模型名。
3. Hook 安装、启用、信任和实际成功触发是不同状态。`doctor` 成功不能证明 Hook 已在真实对话中触发。
4. 只通过 Codex 官方插件和 Hook 信任入口操作，不修改信任数据库或绕过信任提示。

## 标题预览与应用

- `rename <话题 ID>` 只生成预览；只有用户明确要求改名时才追加 `--apply`。
- `renamed` 表示标题元数据已写入并读回核验；候选结果本身不代表已经生效。
- `locked` 或 `manual_title` 表示标题受保护。只有用户要求恢复自动命名或覆盖手动标题时才执行 `unlock <话题 ID>`。
- `stale_result` 或 `outdated_event` 表示内容在生成期间变化，本次结果已丢弃；用户要求立即更新时可重新预览。
- `ambiguous_title` 表示标题与同一工作目录中已记录的话题重名，插件保留原名；可根据对话中真实对象补充后再试。

## 控制命令

- `pause` / `resume`：暂停或恢复后续自动命名。写入前会再次检查状态。
- `lock <话题 ID>` / `unlock <话题 ID>`：保护或解除当前标题。
- `status`：显示配置位置和已记录话题数。
- `usage`：查看新版模型调用账本；缓存包含在输入用量中，推理输出包含在输出用量中。

首次处理前无法可靠判断已有标题是否由用户手动设置。需要长期保留的标题请使用 `lock`。

## 隐私与边界

插件读取近期用户请求与最终回答，只把简短项目线索（不含绝对路径）交给当前登录的 Codex 模型。后台通过官方标题接口写入元数据，不恢复原话题、不发送新消息、不直接改写数据库。持久记录保存状态、标题和用量，不保存完整对话。
