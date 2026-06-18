# 终端环境优化总结

> 基于现代 CLI 工具栈与插件，对 zsh + mise + tmux 进行全面优化，提升终端使用效率与体验。

---

## 一、Zsh Shell 优化

### 1.1 工具栈

| 工具 | 用途 | 安装方式 | 状态 |
|------|------|------|:----:|
| oh-my-zsh | shell管理 | curl  | ✅ |
| mise | 工具管理 | apt/curl | ✅ |
| Starship | 现代化 Prompt | mise | ✅ |
| zoxide | 智能 cd 替代 | mise | ✅ |
| fzf | 模糊搜索 | mise | ✅ |
| eza | ls 替代 | mise | ✅ |
| fd | find 替代 | mise | ✅ |
| ripgrep | grep 替代 | mise | ✅ |
| bat | cat 替代 | mise | ✅ |
| tmux | 会化管理 | mise | ✅ |
| oh-my-tmux | tmux体验优化 | curl | ✅ |

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

`~/.config/starship.toml` 采用 gruvbox-rainbow 风格

---

## 二、Tmux 优化

### 2.1 基本信息

- **上游**：[gpakosz/.tmux](https://github.com/gpakosz/.tmux)（Oh my tmux!）
- **安装**：一键脚本，`~/.tmux.conf` 为上游 symlink，自定义在 `~/.tmux.conf.local`
- **Prefix 键**：`Ctrl+B`（主）+ `Ctrl+A`（副）
- **插件管理器**：TPM（Oh my tmux! 内置集成）

### 2.2 安装 Oh my tmux
```bash
# 1. 执行安装命令
curl -fsSL "https://github.com/gpakosz/.tmux/raw/refs/heads/master/install.sh#$(date +%s)" | bash

# 2. 重新加载 zsh 配置
exec zsh

# 3. 启动 tmux，安装插件：Prefix + I
```

### 2.3 插件列表

| 插件 | 功能 |
|------|------|
| `tmux-plugins/tmux-sensible` | 开箱即用的合理默认值 |
| `tmux-plugins/tmux-yank` | 系统剪贴板集成 |
| `tmux-plugins/tmux-resurrect` | 会话持久化保存/恢复 |
| `tmux-plugins/tmux-continuum` | 自动保存（每 15 分钟） |

### 2.4 快捷键

#### 窗格（Pane）操作

| 按键 | 功能 | 来源 |
|------|------|------|
| `Prefix + _` | 垂直分割（`split-window -h`，左右分栏，保留目录） | Oh My Tmux |
| `Prefix + -` | 水平分割（`split-window -v`，上下分栏，保留目录） | Oh My Tmux |
| `Prefix + h/j/k/l` | 切换到 左/下/上/右 窗格（Vim 风格） | Oh My Tmux |
| `Prefix + H/J/K/L` | 将窗格向 左/下/上/右 扩大 | Oh My Tmux |
| `Prefix + +` | 最大化/还原当前窗格（独立窗口模式） | Oh My Tmux |
| `Prefix + <` / `>` | 窗格位置左/右交换 | Oh My Tmux |
| `Prefix + x` | 关闭当前窗格 | tmux 原生 |
| `Prefix + z` | 原生最大化/还原（`resize-pane -Z`） | tmux 原生 |
| `Prefix + {` / `}` | 窗格位置左/右交换（原生方式） | tmux 原生 |
| `Prefix + Space` | 切换窗格布局 | tmux 原生 |

#### 窗口（Window）操作

| 按键 | 功能 | 来源 |
|------|------|------|
| `Prefix + c` | 新建窗口 | tmux 原生 |
| `Prefix + ,` | 重命名窗口 | tmux 原生 |
| `Prefix + C-h` / `C-l` | 上一个/下一个窗口（Oh My Tmux 替代原生 `n`/`p`） | Oh My Tmux |
| `Prefix + Tab` | 切换到上一个活跃窗口 | Oh My Tmux |
| `Prefix + 1-9` | 跳转到指定编号窗口 | tmux 原生 |
| `Prefix + &` | 关闭当前窗口 | tmux 原生 |

#### 会话（Session）操作

| 按键 | 功能 | 来源 |
|------|------|------|
| `Prefix + C-c` | 新建会话 | Oh My Tmux |
| `Prefix + C-f` | 按名称查找并切换会话 | Oh My Tmux |
| `Prefix + s` | 会话树（交互式选择） | tmux 原生 |
| `Prefix + w` | 窗口树预览 | tmux 原生 |
| `Prefix + BTab` | 切换到上一个会话 | Oh My Tmux |

#### 复制模式（Vi 风格）

| 按键 | 功能 | 来源 |
|------|------|------|
| `Prefix + Enter` | 进入复制模式 | Oh My Tmux |
| `v` | 开始选择（visual） | Oh My Tmux |
| `y` | 复制到粘贴缓冲区 | Oh My Tmux |
| `Ctrl+r` | 反向搜索 | tmux 原生 |

#### 其他

| 按键 | 功能 | 来源 |
|------|------|------|
| `Prefix + e` | 在编辑器中打开 `.tmux.conf.local` | Oh My Tmux |
| `Prefix + r` | 重载 tmux 配置 | Oh My Tmux |
| `Prefix + m` | 切换鼠标模式 | Oh My Tmux |
| `Prefix + I` | 安装 TPM 插件 | TPM 插件 |
| `Prefix + u` | 更新 TPM 插件 | TPM 插件 |
| `Prefix + d` | 分离会话 | tmux 原生 |
| `C-l`（无需 Prefix） | 清屏并清除回滚历史 | Oh My Tmux |
| 鼠标点击 | 切换窗格 / 选择窗口 / 拖拽 / 滚动 | tmux 原生 |

### 2.5 通用配置

| 设置 | 值 | 说明 |
|------|-----|------|
| `history-limit` | 50000 | 回滚缓冲区行数 |
| `escape-time` | 10ms | 加快 Esc 响应（vi-mode 兼容） |
| `base-index` | 1 | 窗口编号从 1 开始 |
| `renumber-windows` | on | 关闭窗口后自动重新编号 |
| `mouse` | on | 全局鼠标支持（Prefix + m 切换） |
| `focus-events` | on | 聚焦事件传递 |
| `default-terminal` | `tmux-256color` | 256 色 + 真彩色 |
| 会话恢复 | on | continuum 自动保存 + 启动恢复 |

---

## 三、修改的文件

| 文件 | 变更 |
|------|------|
| `~/.zshrc` | 重写，包含插件配置与工具别名 |
| `~/.config/bat/config` | 新建，主题 Catppuccin Mocha |
| `~/.config/mise/config.toml` | 新增 `zoxide = "latest"` |
| `~/.tmux/` | Oh my tmux! 一键安装 |
| `~/.tmux.conf` | install.sh 自动创建 symlink |
| `~/.tmux.conf.local` | 自定义配置（主题 / 插件 / 快捷键） |


