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
| Agent turn completed | Complete sound |
| Permission request | Permission sound |
| Session error or retry | Retry sound |

### Requirements

- Windows
- Python 3.10 or later (ensure `python` is added to your system's PATH environment variable)
- Codex CLI with Hooks support

### Repository Layout

```text
codex-sound-notifier/
├── codex_ring.py
├── hooks.json
├── README.md
└── .gitignore
```

`codex_ring.py` and `hooks.json` are stored in the same directory in this repository.

### Installation

1. Copy `codex_ring.py` and `hooks.json` to the same local directory:

   ```text
   %USERPROFILE%\.codex\
   ```

2. Replace `YOUR_USERNAME` in `hooks.json` with your Windows user name. Hook commands must use an absolute path because Codex may run from any workspace directory.

3. Add the following line near the top of `%USERPROFILE%\.codex\config.toml`:

   ```toml
   notify = ["python", "C:\\Users\\YOUR_USERNAME\\.codex\\codex_ring.py"]
   ```

4. Restart Codex.

5. If Codex asks whether to trust the local hooks, approve the hooks after reviewing the files.

### Hook Configuration

The hook configuration uses:

- `PermissionRequest` for permission notification sounds.
- `SessionStart` to start the structured session-error watcher.

### Testing

Play the embedded sounds directly:

```cmd
python "%USERPROFILE%\.codex\codex_ring.py" play-audio complete
python "%USERPROFILE%\.codex\codex_ring.py" play-audio permission
python "%USERPROFILE%\.codex\codex_ring.py" play-audio retry
```

### Notes

- The completion notification uses Codex `notify` with the `agent-turn-complete` event.
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
├── README.md
└── .gitignore
```

仓库中的 `codex_ring.py` 与 `hooks.json` 位于同一目录。

### 安装步骤

1. 将 `codex_ring.py` 与 `hooks.json` 复制到同一目录：

   ```text
   %USERPROFILE%\.codex\
   ```

2. 将 `hooks.json` 中的 `YOUR_USERNAME` 替换为你的 Windows 用户名。Hook 命令必须使用绝对路径，因为 Codex 可能在任意项目目录中运行。

3. 在 `%USERPROFILE%\.codex\config.toml` 顶部附近添加：

   ```toml
   notify = ["python", "C:\\Users\\你的用户名\\.codex\\codex_ring.py"]
   ```

4. 重启 Codex。

5. 如果 Codex 首次提示是否信任本地 Hook，请先检查脚本内容，再选择信任。

### Hook 配置

- `PermissionRequest`：用于权限请求提示音。
- `SessionStart`：用于启动结构化会话错误监听器。

### 测试方法

可以直接播放内嵌音频：

```cmd
python "%USERPROFILE%\.codex\codex_ring.py" play-audio complete
python "%USERPROFILE%\.codex\codex_ring.py" play-audio permission
python "%USERPROFILE%\.codex\codex_ring.py" play-audio retry
```

### 注意事项

- 任务完成通知依赖 Codex `notify` 的 `agent-turn-complete` 事件。
- 错误提示通过解析结构化 JSONL 会话记录实现。
- 本项目不会自动批准、拒绝或修改任何 Codex 权限请求。
- 由于内嵌了 Base64 音频，`codex_ring.py` 文件体积会较大。
