## 在Ubuntu 18.04 LTS上GLIBCXX_3.4.26缺失：

./AppOFCuda: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.26' not found (required by ./AppOFCuda)

这个错误提示表明你的系统中的 `libstdc++.so.6` 库版本过低，你的应用 `AppOFCuda` 需要版本 `GLIBCXX_3.4.26`。你可以通过以下步骤更新你的 `libstdc++.so.6`：

1. 打开终端，查看当前的 `libstdc++.so.6` 支持的 GLIBCXX 版本：

```
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_3.4.18
GLIBCXX_3.4.19
GLIBCXX_3.4.20
GLIBCXX_3.4.21
GLIBCXX_3.4.22
GLIBCXX_3.4.23
GLIBCXX_3.4.24
GLIBCXX_3.4.25
GLIBCXX_DEBUG_MESSAGE_LENGTH
```

2. 如果在列出的版本中不存在 `GLIBCXX_3.4.26`，那么你可能需要更新你的 GCC。在 Ubuntu 中，你可以通过添加 Ubuntu 工具链测试库（toolchain test）PPA 来获得更新的 GCC 版本：

添加 PPA：

```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt upgrade libstdc++6
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```