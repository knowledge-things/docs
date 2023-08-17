# [Ubuntu编译生成libevent库]

```
cmake .. -DENABLE_VISION=ON -DENABLE_HTTP=ON -DCMAKE_INSTALL_PREFIX=${PWD}/installed_fastdeploy
make libevent
```

报错信息

```
CMake Error at /usr/share/cmake-3.16/Modules/FindPackageHandleStandardArgs.cmake:146 (message):
  Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the
  system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY
  OPENSSL_INCLUDE_DIR)
Call Stack (most recent call first):
  /usr/share/cmake-3.16/Modules/FindPackageHandleStandardArgs.cmake:393 (_FPHSA_FAILURE_MESSAGE)
  /usr/share/cmake-3.16/Modules/FindOpenSSL.cmake:447 (find_package_handle_standard_args)
  CMakeLists.txt:683 (find_package)
```

## 解决方法：

### 确保已安装 OpenSSL 库

```
openssl version
```

需要配置环境变量PKG_CONFIG_PATH为openssl.pc的路径，Ubuntu已经装了openssl，但是**找不到openssl.pc**，原因是**没有安装 libssl-dev**。 

```
brew install openssl
```

