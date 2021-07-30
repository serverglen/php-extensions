- [简介](#简介)
- [编译/安装](#编译安装)
  - [编译安装libxdiff库](#编译安装libxdiff库)
    - [下载libxdiff库](#下载libxdiff库)
    - [解压](#解压)
    - [编译和安装](#编译和安装)
  - [编译xdiff扩展](#编译xdiff扩展)
    - [下载xdiff扩展源码](#下载xdiff扩展源码)
    - [解压](#解压-1)
    - [修改config.m4文件](#修改configm4文件)
    - [编译和安装](#编译和安装-1)
- [测试xdiff扩展](#测试xdiff扩展)
  - [修改php.ini文件](#修改phpini文件)
  - [拷贝xdiff扩展](#拷贝xdiff扩展)
  - [验证扩展是否加载成功](#验证扩展是否加载成功)
- [xdiff函数](#xdiff函数)

# 简介
xdiff扩展使您可以创建和应用包含文件不同修订版之间差异的补丁文件。

此扩展支持两种操作模式-字符串和文件，以及两种不同的补丁格式-统一和二进制。统一补丁非常适合文本文件，因为它们易于阅读并且易于查看。对于诸如存档或图像之类的二进制文件，二进制补丁将是适当的选择，因为它们是二进制安全的并且可以很好地处理不可打印的字符。

从1.5.0版开始，有两组不同的函数用于生成二进制补丁。新函数xdiff_string_rabdiff()和xdiff_file_rabdiff()生成与旧函数兼容的输出，但通常更快，并且生成的结果更小。有关生成二进制补丁及其区别的方法的更多详细信息，请访问[libxdiff网站](http://www.xmailserver.org/xdiff-lib.html)。

# 编译/安装

## 编译安装libxdiff库

### 下载libxdiff库
```bash
wget http://www.xmailserver.org/libxdiff-0.23.tar.gz
```
当然也可以使用当前目录下的libxdiff-0.23.tar.gz

### 解压
```bash
tar xzf libxdiff-0.23.tar.gz
```

### 编译和安装
```bash
$ export LIBXDIFF_INSTALL_DIR=you_want_install_libxdiff_dir
$ mkdir -p $LIBXDIFF_INSTALL_DIR
$ cd libxdiff-0.23
$ CFLAGS="-fPIC" ./configure --prefix=$LIBXDIFF_INSTALL_DIR --enable-shared=no --enable-static=yes
$ make -j4
$ make install
```

## 编译xdiff扩展

### 下载xdiff扩展源码
```bash
wget https://pecl.php.net/get/xdiff-2.1.0.tgz
```
当然也可以直接使用当前目录下的xdiff-2.1.0.tgz

* pecl xdiff 扩展地址：https://pecl.php.net/package/xdiff
* xdiff github 地址： github.com/php/pecl-text-xdiff

注意：主要使用最新版本

### 解压
```bash
tar xzf xdiff-2.1.0.tgz
```
### 修改config.m4文件
注意：这一步是必须的，否则在有些系统上编译不过去。
将如下两行注释掉，注释符号是：dnl
```bash
PHP_CHECK_LIBRARY(xdiff,xdl_set_allocator, [ ], AC_MSG_ERROR([your libxdiff version is too old]))
PHP_CHECK_LIBRARY(xdiff,xdl_rabdiff, [ ], AC_MSG_ERROR([your libxdiff version is too old]))
```
注释后的效果如下：
```bash
dnl check for xdl_rabdiff function
dnl PHP_CHECK_LIBRARY(xdiff,xdl_set_allocator, [ ], AC_MSG_ERROR([your libxdiff version is too old]))
 
dnl check for xdl_rabdiff function
dnl PHP_CHECK_LIBRARY(xdiff,xdl_rabdiff, [ ], AC_MSG_ERROR([your libxdiff version is too old]))
```
### 编译和安装
```bash
$ export PHP_HOME=you_php_home
$ cd xdiff-2.1.0
$ $PHP_HOME/php/bin/phpize
$ ./configure --with-php-config=$PHP_HOME/php/bin/php-config --with-xdiff=$LIBXDIFF_INSTALL_DIR
$ make -j4
```
执行完了之后，modules目录会产出xdiff.so
```bash
$ ls modules
xdiff.so
```
# 测试xdiff扩展
## 修改php.ini文件
在php.ini文件末尾增加如下内容：
```bash
[xdiff]
extension="xdiff.so"
```
## 拷贝xdiff扩展
将编译好的xdiff.so拷贝到php.ini文件中extension_dir配置的位置。
比如我的php.ini中extension_dir配置如下：
```bash
extension_dir="/home/work/php/ext"
```
则只需要将编译好的xdiff.so拷贝到/home/work/php/ext目录
```bash
cp modules/xdiff.so /home/work/php/ext
```
## 验证扩展是否加载成功
```bash
$ cd $PHP_HOME
$ php/bin/php --ri xdiff
 
xdiff
 
xdiff support => enabled
extension version => 2.1.0
libxdiff version => LibXDiff v0.23 by Davide Libenzi <davide@xmailserver.org>
```
如果执行php/bin/php --ri xdiff命令可以输出上述信息，那说明xdiff扩展已经加载成功了。

congratulations！！！

# xdiff函数

* xdiff_file_bdiff_size — Read a size of file created by applying a binary diff
* xdiff_file_bdiff — Make binary diff of two files
* xdiff_file_bpatch — Patch a file with a binary diff
* xdiff_file_diff_binary — Alias of xdiff_file_bdiff
* xdiff_file_diff — Make unified diff of two files
* xdiff_file_merge3 — Merge 3 files into one
* xdiff_file_patch_binary — Alias of xdiff_file_bpatch
* xdiff_file_patch — Patch a file with an unified diff
* xdiff_file_rabdiff — Make binary diff of two files using * the Rabin's polynomial fingerprinting algorithm
* xdiff_string_bdiff_size — Read a size of file created by * applying a binary diff
* xdiff_string_bdiff — Make binary diff of two strings
* xdiff_string_bpatch — Patch a string with a binary diff
* xdiff_string_diff_binary — Alias of xdiff_string_bdiff
* xdiff_string_diff — Make unified diff of two strings
* xdiff_string_merge3 — Merge 3 strings into one
* xdiff_string_patch_binary — Alias of xdiff_string_bpatch
* xdiff_string_patch — Patch a string with an unified diff
* xdiff_string_rabdiff — Make binary diff of two strings using the Rabin's polynomial fingerprinting algorithm
