# Conda虚拟环境下Error: /lib/x86_64-linux-gnu/libp11-kit.so.0: undefined symbol: ffi_type_pointer, version LIBFFI_BASE_7.0

## 1 背景说明

在引入fastdeploy包时报错`RuntimeError: FastDeploy initalized failed! Error: /lib/x86_64-linux-gnu/libp11-kit.so.0: undefined symbol: ffi_type_pointer, version LIBFFI_BASE_7.0`

## 2 报错原因

libffi的版本不一致，导致了libp11-kit.so.0在使用时出现了未定义符号问题

libffi.so.7链接至libffi.so.8.1.0，所以，这也就是为什么会在程序中，libffi报版本错误了。找到原因，解决方法也很简单，我这边选择的方式是将该路径下的libffi.so.7文件备份后（重命名为libffi_bak.so.7），再在该路径下创建一个新的libffi.so.7链接至/lib/x86_64-linux-gnu/libffi.so.7.1.0，即输入命令：

```
sudo ln -s /lib/x86_64-linux-gnu/libffi.so.7.1.0 libffi.so.7
sudo ldconfig
```

