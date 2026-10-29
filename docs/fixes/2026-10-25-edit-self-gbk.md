# edit_self.py：GBK 控制台打印崩，备份却成功了

**日期** 2026-10-25 · **影响** 备份脚本误报失败 · **难度** 低

## 现象

在 Windows 默认控制台（GBK）里跑：

```powershell
python _probe/edit_self.py backup _probe/edit_self.py "改什么"
```

报错：

```
UnicodeEncodeError: 'gbk' codec can't encode character '\u2705' ...
```

退出码 1。

## 根因

- 脚本先 `shutil.copy2` 复制文件，再 `print('  ✅ 备份文件……')`。崩在最后一行。
- 控制台输出编码是 GBK，emoji 编不进去；崩的是 `print`，不是备份。
- **退出码只反映进程最后炸了什么，不反映活干没干。**

## 补丁

加一个安全打印函数，`UnicodeEncodeError` 时退回 ASCII：

```python
def _p(msg):
    try:
        print(msg)
    except UnicodeEncodeError:
        print(msg.encode('ascii', 'replace').decode('ascii'))
```

把带 emoji 的 `print` 都换成 `_p`（共 8 处）。

## 验证

```powershell
$env:PYTHONIOENCODING='gbk'
python _probe/edit_self.py backup _probe/edit_self.py "验证 GBK 回退"
# EXIT=0，备份目录照常生成
```

## 通用教训

**退出码 1 ≠ 活没干。** 看到失败先查产物（备份目录 / 文件），再决定重跑还是修。
