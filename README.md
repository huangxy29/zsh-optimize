# 终端环境优化总结

> 基于现代 CLI 工具栈与插件，对 zsh + tmux 进行全面优化，提升终端使用效率与体验。

---

## 一、Zsh Shell 优化

### 1.1 工具栈

| 工具 | 版本 | 用途 | 状态 |
|------|------|------|:----:|
| Starship | 1.25.1 | 现代化 Prompt | ✅ |
| zoxide | 0.9.9 | 智能 cd 替代 | ✅ |
| fzf | 0.73.1 | 模糊搜索 | ✅ |
| eza | 0.23.4 | ls 替代 | ✅ |
| fd | 10.4.2 | find 替代 | ✅ |
| ripgrep | 15.1.0 | grep 替代 | ✅ |
| bat | 0.26.1 | cat 替代 | ✅ |

### 1.2 插件列表

| 插件 | 来源 | 功能 |
|------|------|------|
| `git` | OMZ 内置 | Git 别名与分支信息 |
| `vi-mode` | OMZ 内置 | Vim 风格模态编辑 |
| `zsh-autosuggestions` | Git clone → OMZ custom | 灰色历史建议（Fish 风格） |
| `history-substring-search` | OMZ 内置 | ↑/↓ / Ctrl+P/N 历史子串匹配 |
| `zoxide` | OMZ 内置 | `z` / `zi` 智能目录跳转 |
| `fzf` | OMZ 内置 | 模糊查找器集成 |
| `zsh-syntax-highlighting` | Git clone → OMZ custom | 命令语法高亮（红/绿） |

### 1.3 快捷键

| 按键 | 功能 |
|------|------|
| `Esc` / `Ctrl+[` | 进入 vi normal 模式 |
| `Ctrl+T` | 模糊文件搜索（bat 预览） |
| `Ctrl+R` | 模糊历史搜索 |
| `Alt+C` | 模糊目录跳转（eza 树预览） |
| `↑/↓` / `Ctrl+P` / `Ctrl+N` | 历史子串搜索 |
| `Ctrl+A` / `Ctrl+E` | 跳转到行首 / 行尾 |
| `vv`（normal 模式） | 在 $EDITOR 中编辑当前命令 |
| `Tab` | fzf 增强补全（cd / ssh / kill 等） |

### 1.4 别名

#### eza（ls 替代）

| 别名 | 等价命令 |
|------|----------|
| `ls` | `eza --group-directories-first --icons` |
| `ll` | `eza --group-directories-first --icons -lagh` |
| `la` | `eza --group-directories-first --icons -a` |
| `lt` | `eza --group-directories-first --icons -T --level=2` |
| `l` | `eza --group-directories-first --icons -l` |

#### bat（cat 替代）

| 别名 | 等价命令 |
|------|----------|
| `cat` | `bat --style=plain --paging=never` |

> 主题：Catppuccin Mocha

#### ripgrep / fd（可选）

```bash
# 如需全局替换，取消 .zshrc 中以下行的注释：
# alias grep='rg'
# alias find='fd'
```

### 1.5 Starship Prompt 配置

`~/.config/starship.toml` 采用 Powerline 风格，但是从第二行开始输入

---

## 二、Tmux 优化

### 2.1 基本信息

- **版本**：tmux 3.6a（via mise）
- **Prefix 键**：`Ctrl+B`（保持默认，兼容 zsh vi-mode 的 `Ctrl+A`）
- **主题**：Catppuccin Mocha（via TPM）
- **插件管理器**：TPM（Tmux Plugin Manager）

### 2.2 插件列表

| 插件 | 功能 |
|------|------|
| `tmux-plugins/tpm` | 插件管理器 |
| `tmux-plugins/tmux-sensible` | 开箱即用的合理默认值 |
| `tmux-plugins/tmux-yank` | 系统剪贴板集成 |
| `tmux-plugins/tmux-resurrect` | 会话持久化保存/恢复 |
| `tmux-plugins/tmux-continuum` | 自动保存（每 15 分钟） |
| `catppuccin/tmux` | Catppuccin Mocha 主题 |

### 2.3 快捷键

#### 窗格（Pane）操作

| 按键 | 功能 |
|------|------|
| `Prefix + \|` | 垂直分割（当前目录） |
| `Prefix + -` | 水平分割（当前目录） |
| `Prefix + h/j/k/l` | 切换到 左/下/上/右 窗格（Vim 风格） |
| `Alt + h/j/k/l` | 同上，无需 Prefix |
| `Prefix + H/J/K/L` | 将窗格向 左/下/上/右 扩大 5 格 |
| `Prefix + Alt + ←↑↓→` | 同上，方向键版本 |
| `Prefix + x` | 关闭当前窗格 |
| `Prefix + z` | 最大化/还原当前窗格 |
| `Prefix + {/}` | 窗格位置左/右交换 |
| `Prefix + Space` | 切换窗格布局 |

#### 窗口（Window）操作

| 按键 | 功能 |
|------|------|
| `Prefix + c` | 新建窗口 |
| `Prefix + ,` | 重命名窗口 |
| `Prefix + n/p` | 下一个/上一个窗口 |
| `Prefix + 1-9` | 跳转到指定编号窗口 |
| `Prefix + &` | 关闭当前窗口 |

#### 会话（Session）操作

| 按键 | 功能 |
|------|------|
| `Prefix + Ctrl+S` | 交互式会话选择器 |
| `Prefix + s` | 会话树（fzf 风格） |
| `Prefix + w` | 窗口树预览 |

#### 复制模式（Vi 风格）

| 按键 | 功能 |
|------|------|
| `Prefix + [` | 进入复制模式 |
| `v` | 开始选择（visual） |
| `y` / `Enter` | 复制到剪贴板 |
| `Ctrl+r` | 反向搜索 |

#### 其他

| 按键 | 功能 |
|------|------|
| `Prefix + r` | 重载 tmux 配置 |
| `Prefix + I` | 安装 TPM 插件（首次使用） |
| `Prefix + U` | 更新 TPM 插件 |
| `Prefix + d` | 分离会话 |
| 鼠标滚轮 | 滚动历史内容 |
| 鼠标拖拽边界 | 调整窗格大小 |

### 2.4 通用配置

| 设置 | 值 | 说明 |
|------|-----|------|
| `history-limit` | 50000 | 回滚缓冲区行数 |
| `escape-time` | 10ms | 加快 Esc 响应（vi-mode 兼容） |
| `base-index` | 1 | 窗口编号从 1 开始 |
| `renumber-windows` | on | 关闭窗口后自动重新编号 |
| `mouse` | on | 全局鼠标支持 |
| `focus-events` | on | 聚焦事件传递 |
| `default-terminal` | `tmux-256color` | 256 色 + 真彩色 |

---

## 三、修改的文件

| 文件 | 变更 |
|------|------|
| `~/.zshrc` | 重写，包含插件配置与工具别名 |
| `~/.config/starship.toml` | 新建，Powerline 风格 Prompt 配置 |
| `~/.config/bat/config` | 新建，主题 Catppuccin Mocha |
| `~/.config/mise/config.toml` | 新增 `zoxide = "latest"` |
| `~/.tmux.conf` | 新建，TPM 插件 + Vim 键位 + Catppuccin 主题 |
| `~/.tmux/plugins/tpm/` | 新建，Tmux Plugin Manager |

---

## 四、生效方法

```bash
# 重新加载 zsh 配置
exec zsh

# 启动 tmux（首次需安装插件：Prefix + I）
tmux
```

或打开一个新的终端窗口。
