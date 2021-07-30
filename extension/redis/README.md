# 简介
phpredis扩展提供了一个与Redis键值存储通信的API。

# 编译

## 下载redis扩展
```
wget https://pecl.php.net/get/redis-5.3.4.tgz
```
当然也可以使用当前目录下的redis-5.3.4.tgz
* pecl redis扩展地址：https://pecl.php.net/package/redis
* redis github地址：https://github.com/phpredis/phpredis

## 解压
```
tar xzf redis-5.3.4.tgz
```

## 编译
```
$ cd redis-5.3.4
$ export PHP_HOME=you_php_home
$ $PHP_HOME/php/bin/phpize
$ ./configure --with-php-config=$PHP_HOME/php/bin/php-config
$ make -j12
```
执行完了之后，modules目录会产出xdiff.so
```
$ ls modules/
redis.so
```

## 测试redis扩展

### 修改php.ini文件
在php.ini文件末尾增加如下内容：
```
[redis]
extension="redis.so"
```
### 拷贝redis扩展
将编译好的redis.so拷贝到php.ini文件中extension_dir配置的位置。 比如我的php.ini中extension_dir配置如下：
```
extension_dir="/home/work/php/ext"
```
则只需要将编译好的redis.so拷贝到/home/work/php/ext目录
```
cp modules/redis.so /home/work/php/ext
```

### 验证扩展是否加载成功
```
$ cd $PHP_HOME
$ php/bin/php --ri redis

redis

Redis Support => enabled
Redis Version => 5.3.4
Redis Sentinel Version => 0.1
Available serializers => php, json

Directive => Local Value => Master Value
redis.arrays.algorithm => no value => no value
redis.arrays.auth => no value => no value
redis.arrays.autorehash => 0 => 0
redis.arrays.connecttimeout => 0 => 0
redis.arrays.distributor => no value => no value
redis.arrays.functions => no value => no value
redis.arrays.hosts => no value => no value
redis.arrays.index => 0 => 0
redis.arrays.lazyconnect => 0 => 0
redis.arrays.names => no value => no value
redis.arrays.pconnect => 0 => 0
redis.arrays.previous => no value => no value
redis.arrays.readtimeout => 0 => 0
redis.arrays.retryinterval => 0 => 0
redis.arrays.consistent => 0 => 0
redis.clusters.cache_slots => 0 => 0
redis.clusters.auth => no value => no value
redis.clusters.persistent => 0 => 0
redis.clusters.read_timeout => 0 => 0
redis.clusters.seeds => no value => no value
redis.clusters.timeout => 0 => 0
redis.pconnect.pooling_enabled => 1 => 1
redis.pconnect.connection_limit => 0 => 0
redis.pconnect.echo_check_liveness => 1 => 1
redis.pconnect.pool_pattern => no value => no value
redis.session.locking_enabled => 0 => 0
redis.session.lock_expire => 0 => 0
redis.session.lock_retries => 10 => 10
redis.session.lock_wait_time => 2000 => 2000
```
如果执行php/bin/php --ri redis命令可以输出上述信息，那么说明redis扩展已经安装成功了。

# 如何使用
```php
<?php

$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$redis->set('mykey', 'myval');
$val = $redis->get('mykey');
```

# redis命令参考
* [redis命令参考](http://doc.redisfans.com/)
