# 定时任务配置说明

## 作用

每天定时把当天新增的存档 md 文件提炼成语义索引，写入 `INDEX-SEMANTIC.md`。这一步**不在对话中做**，由定时任务在后台批量跑。

## 通用做法

### 方案 A：用 AI 助手自带的定时任务能力

如果你用的 AI 助手支持定时任务（如豆包工作的 cron），创建一个每天定时触发的任务，query 写：

> 本次请求是由「每日索引提炼」定时任务到时触发的。执行以下操作：
> 1. 扫描记忆库目录下当前年月文件夹中今天新增或修改的存档 md 文件；
> 2. 读取每个文件，提炼：日期、主题、一句话摘要、文件路径、关键词（3-5个）；
> 3. 追加到 `INDEX-SEMANTIC.md`（格式：日期 | 主题 | 一句话摘要 | 文件路径 | 关键词）；
> 4. 备份 `windows-map.json` 为 `windows-map.bak.json`；
> 5. 幂等：已在索引中出现过的文件路径不重复追加。

推荐时间：**每天 17:30**（下班前，当天对话基本结束后）。

### 方案 B：系统级定时任务

如果你不想依赖 AI 助手的定时能力，可以写一个脚本：

```python
# index_builder.py（伪代码示意）
import os, glob, datetime

MEMORY_ROOT = "/path/to/your-memory"
today = datetime.date.today().isoformat()

# 扫今天修改过的 md
for md in glob.glob(f"{MEMORY_ROOT}/2026/*/*.md"):
    mtime = datetime.date.fromtimestamp(os.path.getmtime(md)).isoformat()
    if mtime == today:
        # 读文件 → 调用 LLM API 提炼摘要 → 追加到 INDEX-SEMANTIC.md
        ...
```

配合 cron（Linux）或任务计划程序（Windows）每天定时跑。

## 索引格式

`INDEX-SEMANTIC.md` 每条一行：

```
2026-10-30 | 流程指标体系设计 | 为三大流程设计16项可度量指标，三率导向 | 2026/10/2026-10-30-流程指标体系.md | 指标,流程,IPD,LTC
```

## 幂等性

定时任务必须检查：文件路径已在索引中出现过的不重复追加。否则每天会重复写入。
