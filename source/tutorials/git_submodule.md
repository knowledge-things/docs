## git 添加子模块

要在你的项目中添加一个子模块，可以按照以下步骤操作。这些步骤将指导你如何将 `https://github.com/RicherMans/CED.git` 添加到你的项目的 `third_party` 目录中。

### 步骤1：导航到你的项目目录

首先，确保你在你的项目的根目录下：

```
sh
复制代码
cd /path/to/your/project
```

### 步骤2：添加子模块

使用 `git submodule add` 命令将子模块添加到 `third_party` 目录中：

```
sh
复制代码
git submodule add https://github.com/RicherMans/CED.git third_party/CED
```

### 步骤3：初始化和更新子模块

添加子模块后，需要初始化并更新子模块：

```
sh
复制代码
git submodule update --init --recursive
```

### 步骤4：提交子模块更改

将子模块的更改提交到你的主仓库：

```
sh
复制代码
git add .gitmodules third_party/CED
git commit -m "Add CED as a submodule in third_party directory"
git push
```

### 验证

确保子模块添加成功，可以通过以下命令查看子模块的状态：

```
sh
复制代码
git submodule status
```

### 全部命令总结

以下是所有命令的总结，按顺序执行：

```
sh
复制代码
# 导航到项目根目录
cd /path/to/your/project

# 添加子模块
git submodule add https://github.com/RicherMans/CED.git third_party/CED

# 初始化并更新子模块
git submodule update --init --recursive

# 提交子模块更改
git add .gitmodules third_party/CED
git commit -m "Add CED as a submodule in third_party directory"
git push

# 查看子模块状态
git submodule status
```

通过这些步骤，你就可以成功地将 `https://github.com/RicherMans/CED.git` 作为子模块添加到你的项目的 `third_party` 目录中，并确保所有更改都提交到你的主仓库。