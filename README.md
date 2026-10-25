# 词序 CiXu v1.5

Windows 词达人任务桌面客户端，基于 [ularch/Easy_Cidaren](https://github.com/ularch/Easy_Cidaren) 修改。支持 Token 获取、任务多选队列、停止与切换、平滑滚动、本地答案匹配和可选模型辅助。

**默认关闭模型，无须 DeepSeek API Key。** 查询课程、获取题目和提交答案仍需联网。本地匹配不保证所有题型均可解答，也不保证分数。

## 从源码运行

建议 Windows 10/11、Python 3.13 x64。在整个项目解压目录打开 PowerShell：

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe main.py
```

保留附带的 `en_core_web_sm` 模型目录。首次启动会从 `config/config.example.json` 创建本机配置。

如使用发布者另行提供的 Windows 程序包，完整解压后运行 `CiXu.exe`，保留 `_internal` 和 `config`。本源码包不含 EXE。

## 获取 Token 与连接

1. 点击“获取 Token”，选择“开始获取”。
2. 将自动复制的学生端链接粘贴到**电脑微信**并打开，在词达人中切换页面。
3. 成功后自动填入并连接。也可粘贴自己的 Token，手动连接账号。
4. 如果异常退出后网络受影响，在获取窗口点击“恢复捕获设置”。

捕获会临时修改当前用户系统代理、添加临时证书，仅解密 `app.vocabgo.com` 的请求以获取 UserToken。成功、取消或超时后恢复设置。Token 仅用于本次会话，不保存到配置。只连接本人有权使用的账号。

## 选择与运行任务

1. 连接后刷新任务列表，勾选一个或多个项目。
2. 点击“开始所选任务”，按队列顺序运行。
3. 切换前点击“停止”，等待当前请求结束、界面恢复可操作后，再选择新任务开始。
4. 运行设置中的“不确定题处理”默认暂停；可按需要人工处理，或选择跳过并记录。

模型辅助保持“关闭”即可使用本地匹配。可选模型需要自行填写服务地址、模型名和 Key。Windows 使用当前用户绑定的 DPAPI 加密保存 Key；**加密配置仍是私密文件，不得上传**。启用模型会将必要题目内容发送给所配置的服务。

## 常见问题

| 现象 | 处理 |
| --- | --- |
| Token 失效或任务加载失败 | 重新获取 Token，连接后刷新 |
| 要求安全验证 | 在官方微信端完成验证后重试 |
| 停止未立即结束 | 等待正在进行的请求返回或超时 |
| 未解题多 | 查看运行记录与 diagnostics/unresolved.jsonl，确认题型；不能保证全部可解 |
| 缓存为空 | 公开版不携带个人学习记录，使用中逐步积累 |

日志经过尽力脱敏，仍可能有课程或题目信息。反馈时只提供必要片段，先自行检查，勿上传整个运行目录。

## 构建程序

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

脚本安装依赖、运行测试并打包到 `dist/CiXu/`，只复制公开默认配置并创建空学习缓存，不复制本机密钥。

## 上传 GitHub 前

```powershell
.\.venv\Scripts\python.exe tools/audit_public.py .
git status --short
git diff --cached
```

上传本公开源码副本，不上传原桌面运行目录或旧程序压缩包。`.gitignore` 排除本机配置、密钥、证书、日志、学习缓存和构建文件；它无法清理已经提交的内容或历史。本副本不含旧 `.git` 历史。详见 [安全检查说明](SECURITY_AUDIT.md)。

## 改进与适用范围

v1.5 改进释义格式、词性、否定、词形还原、例句填空和缓存匹配。历史 180 道文本题快照中可生成本地答案由 173 道增至 180 道，其中具有历史判分的 69 道与正确答案一致。该样本不代表新题或所有题型的正确率。

## 来源与许可

独立修改版，非上游官方发行。来源和修改说明见 [NOTICES.md](NOTICES.md)，原始说明见 README-upstream.MD。采用 GPL-3.0，完整许可见 LICENSE，第三方许可见 THIRD-PARTY-LICENSES。分发程序时同时提供对应修改源码和许可说明。
