# DSH Fixes

DeepSeek Harness 的修复案例库。每个案例写清四件事：**现象 · 根因 · 补丁 · 验证**。

> 不收"我觉得应该"，只收"我修过、验过"。

## 案例

- [2026-10-25 · edit_self.py 在 GBK 控制台打印崩掉，备份却成功了](docs/fixes/2026-10-25-edit-self-gbk.md)
- [2026-10-25 · write 全文重写会作废之前的 read（FS_NOT_OBSERVED）](docs/fixes/2026-10-25-write-invalidates-read.md)
- [2026-10-25 · 备份机制复盘：三路备份，三处坑](docs/fixes/2026-10-25-backup-mechanism.md)

## 更新详情

每次修了什么，按日期记在 [CHANGELOG.md](CHANGELOG.md)。

## 投稿

修完一个坑，按 [`docs/fixes/_template.md`](docs/fixes/_template.md) 写一页，PR 到这个仓库。要求：现象可复现、根因指到源码或实测、补丁能直接看、验证是跑过的不只是想过的。

详见 [CONTRIBUTING.md](CONTRIBUTING.md)。
