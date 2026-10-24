# Git Sync Hub v4.0 中文图解与 FDE 操作教程

> **适用对象：** 想了解 `mqgg5630-cyber/leanrea` 分支中 `skills/git-sync` / `git-sync-hub` 的用户与 FDE 工程师。  
> **文档性质：** 根据公开仓库页面与源文件静态整理；不是安装结果，也不是对 `LAPTOP-R77M5D6M` 的连接证明。

## 交付物

- `git-sync-hub-v4-zh-cn.pdf`：中文教程（8 页）。
- `git-sync-hub-v4-cn-package.zip`：打包后的教程、5 张示意图、来源审查和 FDE Skill。
- `assets/`：PDF 中的 5 张操作流程示意图。每张图都标记“非本机真实截图”。
- 本说明与 `source-review.md`：依据、限制与风险核对。

## 这个产品是什么

链接中的 `leanrea` 分支包含两类不同内容：

1. `leanrea/`：面向逆向分析工作流的 Skill。
2. `skills/git-sync/`：Git 远端分支驱动的本机同步/任务派发工具说明与脚本。仓库 README 将其描述为可向 Windows watcher 派发文件写入、复制或 PowerShell / Python / Bash / CMD 命令，并把回执推回 Git。

这类架构不只是“文件同步”：如果本机 watcher 信任远端任务，它实际上也是一个可执行任务的远程代理。公开仓库的说明和回执声称本机服务已启动，但本次会话没有连接用户设备，无法独立确认该服务当前是否运行、是否属于用户预期的配置，或回执是否仍然有效。

## 图解阅读方式

PDF 内图像是**基于公开页面内容绘制的界面/流程示意**，不是来自用户电脑的实拍截图，不是运行成功的证明。具体源码位置与链接见 `source-review.md`。

## 建议的 FDE 使用流程

1. **锁定目标**：核对仓库、分支、提交 SHA、预期主机和允许访问的路径；不要只凭 README 声称的主机名推断连接已建立。
2. **先做静态审查**：阅读安装器、任务派发脚本、watcher 和清理逻辑；确认哪些字段会被执行，哪些目录会被读写，结果如何回传。
3. **验证本机状态**：由设备所有者在本机打开“任务计划程序”→“任务计划程序库”，查找 `git-sync-hub`，核对状态、上次运行时间与“操作”页中的脚本路径；再核对当前 Git 分支与日志。不要仅因任务名显示 Running 就认定配置可信。没有本机证据时，状态标为“未核验”。
4. **限制范围**：若用户自行启用，使用专用测试仓库、单一测试目录和最小权限；不要向 watcher 投递含密钥或任意命令的任务。
5. **核对回执**：检查回执中的主机、时间、分支、源提交、执行项、复制目标和结果目录是否互相吻合；只把可复核回执描述为证据，不把“README 写着 PASS”当成实时验证。
6. **归档交付**：把教程、示意图、PDF 放在同一命名文件夹；写清版本、来源和生成日期，不覆盖旧版。

## 安全边界

- `powershell`、`python`、`bash`、`cmd` 字段可承载可执行代码；若远端 watcher 自动接受它们，推送者可能获得本机代码执行能力。
- “静默”“零弹窗”“自动轮询所有 `arena/*` 分支”并不等于安全，也不证明设备仍处于用户控制之下。
- 不要向 Git 分支提交待执行的任务文件来测试本机是否在线；这会将代码/写盘行为交给本机 watcher。
- 本会话不能读取或写入用户电脑的 `C:`、`D:`、`E:` 盘。GitHub 推送或 Drive 上传都不能证明文件已落到这些盘。

## 资料来源

- 仓库分支：<https://github.com/mqgg5630-cyber/leanrea/tree/arena%2F626378d6-leanrea>
- 主 README：<https://github.com/mqgg5630-cyber/leanrea/blob/arena/626378d6-leanrea/README.md>
- Skill：<https://github.com/mqgg5630-cyber/leanrea/blob/arena/626378d6-leanrea/skills/git-sync/SKILL.md>
- 派发脚本：<https://github.com/mqgg5630-cyber/leanrea/blob/arena/626378d6-leanrea/skills/git-sync/scripts/agent-hub.sh>
- 仓库自述的准备回执：<https://github.com/mqgg5630-cyber/leanrea/blob/arena/626378d6-leanrea/results/cleanup/PERMANENT_HUB_READY.md>
