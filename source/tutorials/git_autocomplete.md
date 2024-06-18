# git autocomplete
在处理源代码时，git版本控制系统是开发人员的热门选择。默认情况下，Git会自动安装在每台Mac上，但您可能希望启用*git选项卡自动完成*功能，以帮助您自动完成命令和分支名称。

如果您使用长分支名称，此功能是必备的。例如，如果您键入`git checkout ma`，然后按Tab键，git tab autocomplete将自动填充分支名称的其余部分，例如：`git checkout main`。

按照本教程中的说明，在Mac上启用git标签自动完成。

 **提示：**不确定您的Mac正在使用什么外壳？您可以使用[“如何判断您的Mac正在使用什么外壳](https://www.macinstruct.com/tutorials/how-to-tell-what-shell-your-mac-is-using/)”中的说明进行检查。

## 为Zsh启用Git Tab自动完成

新Mac默认使用Zsh shell。如果您使用的是Zsh，请将以下行添加到`~/.zshrc`文件中，然后重新启动终端应用程序：

```
autoload -Uz compinit && compinit
```

或者，您可以在终端应用程序中运行以下两个命令，将必要的行添加到`.zshrc`文件中，然后重新启动shell。

```zsh
echo 'autoload -Uz compinit && compinit' >> ~/.zshrc
source ~/.zshrc
```

Git选项卡自动完成现在在Mac上启用。

## 为Bash启用Git Tab自动完成

如果您的[Mac设置为使用Bash shell](https://www.macinstruct.com/tutorials/how-to-set-bash-as-the-default-shell-on-mac/)，请按照以下说明在Mac上启用git tab自动完成：

1. 使用以下curl命令将必要的脚本下载到您的Mac上：

   ```bash
   curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
   ```

2. 将以下行添加到`~/.bash_profile`文件中：

   ```
   if [ -f ~/.git-completion.bash ]; then
     . ~/.git-completion.bash
   fi
   ```

3. 通过运行以下命令[使Bash脚本可执行](https://www.macinstruct.com/tutorials/how-to-make-a-bash-script-executable-on-a-mac/)：

   ```bash
   chmod +x ~/.git-completion.bash
   ```

4. 重新启动终端应用程序或运行以下命令：

   ```bash
   source ~/.bash_profile
   ```

Git选项卡自动完成现在在Mac上启用。