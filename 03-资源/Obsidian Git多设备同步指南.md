# Obsidian Git 插件多设备同步指南

> 本指南基于你的仓库：[puzou/obsidian-vault](https://github.com/puzou/obsidian-vault)

---

## 一、当前状态

- ✅ 仓库已创建并推送成功
- ✅ Obsidian Git 插件已安装（v2.38.3）
- ⚠️ 自动同步功能尚未开启（需要配置）

---

## 二、配置 Obsidian Git 自动同步

在 Obsidian 中按 `Ctrl + ,` 打开设置 → 社区插件 → **Git**，找到以下关键配置项：

### 推荐配置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| **Auto backup interval** | `10`（分钟） | 每隔 10 分钟自动 commit + push |
| **Auto pull interval** | `10`（分钟） | 每隔 10 分钟自动 pull |
| **Auto pull on boot** | ✅ 开启 | Obsidian 启动时自动拉取远程更新 |
| **Pull before push** | ✅ 开启 | push 前先 pull，避免冲突 |
| **Sync method** | `merge` | 同步方式用合并（默认） |
| **Commit message** | `vault backup: {{date}}` | 自动提交信息模板 |
| **Show status bar** | ✅ 开启 | 底部显示同步状态 |

> 也可以直接在 Obsidian 中按 `Ctrl + P`，输入 `Git: Open vault backup` / `Git: Create backup` 手动触发。

---

## 三、在第二台设备上同步（核心步骤）

### 方案 A：电脑端（Windows / Mac / Linux）

**第 1 步**：安装 Git 和 Obsidian（如果还没装）

**第 2 步**：克隆仓库

```bash
# 打开终端，选择一个你想放笔记的目录
cd ~/Documents

# 克隆你的仓库
git clone https://github.com/puzou/obsidian-vault.git
```

**第 3 步**：用 Obsidian 打开这个文件夹

- 打开 Obsidian → "打开文件夹作为仓库" → 选择刚克隆的 `obsidian-vault` 文件夹

**第 4 步**：安装社区插件

- 首次打开时，Obsidian 会在安全模式下运行
- 去 `设置 → 社区插件 → 关闭安全模式`
- 插件已在 `.obsidian/plugins/` 里随仓库同步过来了，**直接启用即可**，不需要重新下载

**第 5 步**：配置 Git 插件（同上面的推荐配置）

---

### 方案 B：手机端（iOS / Android）

**第 1 步**：安装 Obsidian Mobile App

**第 2 步**：在手机上克隆仓库

**iOS（使用 Working Copy）**：
1. App Store 下载 [Working Copy](https://apps.apple.com/app/working-copy-git-client/id896694807)
2. 打开 Working Copy → 克隆 `https://github.com/puzou/obsidian-vault.git`
3. 点击仓库 → "Share" → "Send to Obsidian"

**Android（使用 Termux）**：
1. 安装 [Termux](https://f-droid.org/packages/com.termux/)
2. 在 Termux 中：
   ```bash
   pkg install git
   cd /sdcard
   git clone https://github.com/puzou/obsidian-vault.git
   ```
3. 用 Obsidian 打开该目录

**第 3 步**：配置 Git 插件（同上），开启自动 pull/push

---

## 四、日常使用流程

配置好自动同步后，**基本不用手动操作**。流程如下：

```
电脑上写笔记 → 自动 commit & push（每10分钟）
                                    ↓
手机上打开 Obsidian → 自动 pull → 看到最新笔记

手机上修改笔记 → 自动 commit & push
                                    ↓
电脑上 Obsidian → 自动 pull → 看到手机改的内容
```

### 手动操作（可选）

在 Obsidian 中 `Ctrl + P` 打开命令面板：

| 命令 | 作用 |
|------|------|
| `Git: Create backup` | 手动 commit + push |
| `Git: Pull` | 手动拉取远程更新 |
| `Git: Open diff view` | 查看修改了什么 |
| `Git: View file history` | 查看文件的修改历史 |

底部状态栏也会显示同步状态图标，点击可以快速操作。

---

## 五、冲突处理

偶尔两台设备同时修改同一个文件会产生冲突。Obsidian Git 使用 `merge` 策略，大多数时候能自动合并。

**如果出现冲突**：
1. Obsidian 底部状态栏会显示 ⚠️ 提示
2. `Ctrl + P` → `Git: Open diff view` 查看冲突位置
3. 手动选择保留哪部分内容
4. `Git: Create backup` 提交解决后的版本

**减少冲突的技巧**：
- 自动同步间隔不要太长（建议 10 分钟）
- 编辑前先手动 `Git: Pull` 一次
- 不要同时在两台设备编辑同一文件

---

## 六、.gitignore 说明

当前 `.gitignore` 已排除以下内容：

```gitignore
.obsidian/workspace.json        # 窗口布局（每台设备不同，不应同步）
.obsidian/workspace-mobile.json # 移动端布局
.obsidian/cache                 # 缓存文件
.workbuddy/                     # WorkBuddy 工作数据
.DS_Store / Thumbs.db          # 系统临时文件
```

> 如果以后需要新增忽略项，直接编辑仓库根目录的 `.gitignore` 文件。

---

## 七、常见问题

**Q: 推送时弹出 GitHub 登录框？**
A: 需要配置 Git 凭据。推荐用 GitHub CLI 登录：
```bash
gh auth login
```
或在 Windows 上安装 [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager)，之后首次输入一次密码就会记住。

**Q: 手机上无法 push？**
A: 移动端 Git 支持有限，推荐用 Working Copy（iOS）或 Termux（Android），并配置 SSH key 或 PAT。

**Q: 想回退到之前的版本？**
A: `Ctrl + P` → `Git: View file history` → 选择某个历史提交 → 回退。

**Q: 仓库太大 push 很慢？**
A: 笔记都是文本文件，通常不会很大。图片较多的话可以考虑用 Git LFS 或把图片存到图床。
