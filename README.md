# Codex Sound Notifier

[English](#english) | [简体中文](#简体中文)

A local sound notification tool for Codex CLI.

> Unofficial project. This project is not affiliated with or endorsed by OpenAI.

## English

### Features

- Plays a sound when an agent turn is completed.
- Plays a different sound when Codex requests permission.
- Watches Codex session logs and plays an error sound for structured error events.
- Embeds WAV audio as Base64 data, so no external audio file path is required.
- Uses a background Python process to avoid blocking Codex.
- Automatically stops the error watcher when there is no recent session log activity (e.g., idle for 30 minutes).

### Sound Mapping

| Event | Sound |
|---|---|
| Agent turn completed | Stop sound |
| Permission request | Permissionrequest sound |
| Session error or retry | SessionStart sound |

### Requirements

- Windows
- Python 3.10 or later (ensure `python` is added to your system's PATH environment variable)
- Codex CLI with Hooks support

### Repository Layout

```text
codex-sound-notifier/
├── codex_ring.py
├── hooks.json
└── README.md
```

`codex_ring.py` and `hooks.json` are stored in the same directory in this repository.

### Installation

1. Copy `codex_ring.py` and `hooks.json` to the same local directory:

   ```text
   %USERPROFILE%\.codex\
   ```

2. Replace `YOUR_USERNAME` in `hooks.json` with your Windows user name. 
   > **Important**: Hook commands must use an absolute path because Codex may run from any workspace directory. It is highly recommended to use forward slashes (`/`) to avoid JSON/shell escaping errors (e.g., `python C:/Users/YOUR_USERNAME/.codex/codex_ring.py`).

3. Ensure Hooks are enabled globally. Add the following lines to your `%USERPROFILE%\.codex\config.toml` (if not already present):

   ```toml
   [features]
   hooks = true
   ```

4. Restart Codex.

5. **Permanent Trust Setup**: Because the hooks run external scripts, Codex's security system will intercept them initially. 
   - Type `/hooks` in the Codex CLI and press Enter.
   - Review the newly added hooks.
   - Press `t` (Trust) for each of them. 
   - *This saves a unique `trusted_hash` in your `config.toml`. As long as you don't change the path or command in `hooks.json`, you will never be asked to approve them again upon restarting.*

### Hook Configuration

The updated hook configuration uses:

- `Stop` for task completion sounds.
- `PermissionRequest` for permission notification sounds.
- `SessionStart` to start the structured session-error watcher.

### Testing

Play the embedded sounds directly:

```cmd
python "%USERPROFILE%\.codex\codex_ring.py" play-audio stop
python "%USERPROFILE%\.codex\codex_ring.py" play-audio permissionrequest
python "%USERPROFILE%\.codex\codex_ring.py" play-audio sessionstart
```

### Notes

- Error notifications are detected from structured JSONL session records.
- The project does not approve, reject, or modify any Codex permission request.
- The bundled audio data increases the size of `codex_ring.py`.

---

## 简体中文

### 功能

- Agent 回合完成时播放提示音。
- Codex 请求权限时播放不同的提示音。
- 监听 Codex 会话日志，发现结构化错误事件时播放错误提示音。
- WAV 音频以 Base64 形式内嵌，不依赖外部音频文件路径。
- 通过后台 Python 子进程播放，不阻塞 Codex。
- 当会话日志在一段时间内（如 30 分钟）无更新时，错误监听器自动退出。

### 声音映射

| 事件 | 声音 |
|---|---|
| Agent 回合完成 | 完成提示音 |
| 请求权限 | 审批提示音 |
| 会话错误或重试 | 错误提示音 |

### 环境要求

- Windows
- Python 3.10 或更高版本（确保 `python` 已添加到系统环境变量 PATH 中）
- 支持 Hooks 的 Codex CLI

### 仓库结构

```text
codex-sound-notifier/
├── codex_ring.py
├── hooks.json
└── README.md
```

仓库中的 `codex_ring.py` 与 `hooks.json` 位于同一目录。

### 安装步骤

1. 将 `codex_ring.py` 与 `hooks.json` 复制到同一目录：

   ```text
   %USERPROFILE%\.codex\
   ```

2. 将 `hooks.json` 中的 `YOUR_USERNAME` 替换为你的 Windows 用户名。
   > **重要提示**：Hook 命令必须使用绝对路径，因为 Codex 可能在任意项目目录中运行。强烈建议使用正斜杠（`/`）来防止 JSON 或命令行的路径转义错误（例如：`python C:/Users/你的用户名/.codex/codex_ring.py`）。

3. 确保全局已开启 Hooks 功能。在 `%USERPROFILE%\.codex\config.toml` 中添加以下内容（如果还没有的话）：

   ```toml
   [features]
   hooks = true
   ```

4. 重启 Codex。

5. **配置永久信任（一劳永逸）**：由于 Hook 调用了外部脚本，Codex 处于安全考虑会在初始时拦截。
   - 在 Codex 交互界面中输入 `/hooks` 并回车。
   - 检查列表中新增的几个钩子事件。
   - 对它们分别按下 `t` 键 (Trust) 进行完全信任。
   - *此操作会在 `config.toml` 中生成唯一的 `trusted_hash`。只要未来不修改 `hooks.json` 里的路径和命令，重启后通知系统将永久自动生效，不再要求审核。*

### Hook 配置

更新后的 Hook 配置包含以下触发器：

- `Stop`：用于触发任务完成提示音。
- `PermissionRequest`：用于触发权限请求提示音。
- `SessionStart`：用于启动结构化会话错误监听器。

### 测试方法

可以直接播放内嵌音频：

```cmd
python "%USERPROFILE%\.codex\codex_ring.py" play-audio stop
python "%USERPROFILE%\.codex\codex_ring.py" play-audio permissionrequest
python "%USERPROFILE%\.codex\codex_ring.py" play-audio sessionstart
```

### 注意事项

- 错误提示通过解析结构化 JSONL 会话记录实现。
- 本项目不会自动批准、拒绝或修改任何 Codex 权限请求。
- 由于内嵌了 Base64 音频，`codex_ring.py` 文件体积会较大。