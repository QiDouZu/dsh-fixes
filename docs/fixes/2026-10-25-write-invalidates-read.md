# write 全文重写后，edit 报 file has not been read（FS_NOT_OBSERVED）

**日期** 2026-10-25 · **影响** 改文件被拒 · **难度** 低

## 现象

先 `read SPONSOR.md`，再 `write` 全文重写，接着 `edit` 想改一处：

```
Error: file has not been read ...
```

## 根因

- DSH 的 fs-observation-policy：`edit` 要求**当前版本**被观测过。
- `write` 全文重写会产生新版本，之前那次 `read` 的观测作废。
- 和 `FS_STALE_VERSION` 不是一回事：后者是**外部进程**改了文件，提示 re-read。

## 补丁 / 做法

- 改已有文件：只走 `read → edit`，不先 `write`。
- `write` 只用于新建文件。
- 如果已经 `write` 了：重新 `read` 一次，再 `edit`。

## 验证

| 操作 | 结果 |
|---|---|
| `read` 后直接 `edit` | ✅ 成功 |
| `write` 后不 `read` 就 `edit` | ❌ FS_NOT_OBSERVED |
| 重新 `read` 后 `edit` | ✅ 成功 |

## 通用教训

**两个"没读过"要分清：**
- `FS_NOT_OBSERVED` = 我自己的 `write` 把观测作废了
- `FS_STALE_VERSION` = 别人改了文件，要重新读
