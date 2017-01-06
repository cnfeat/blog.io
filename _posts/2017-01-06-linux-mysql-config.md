---
layout: post
title: mySQL优化及配置说明
date: 2017-01-06
categories: blog
tags: [mysql]
description: mySQL优化及配置说明
---

通过yum安装后，默认配置文件位置：`/etc/my.cnf`

默认模板配置文件位置：`/usr/local/mysql/support-files/`

具体配置文件内容如下：

    [mysqld]
    port = 3306
    serverid = 1
    socket = /tmp/mysql.sock
 
    skip-name-resolve       #禁止MySQL对外部连接进行DNS解析
    skip-grant-tables
    #禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。但需要注意，如果开启该选项，则所有远程主机连接授权都要使用IP地址方式，否则MySQL将无法正常处理连接请求！注：如果用winform连接mysql，加入此句速度会有很大的提升
    skip-external-locking
    # 避免MySQL的外部锁定，减少出错几率增强稳定性。
    back_log = 384
    指定MySQL可能的连接数量。当MySQL主线程在很短的时间内接收到非常多的连接请求，该参数生效，主线程花费很短的时间检查连接并且启动一个新线程。 back_log参数的值指出在MySQL暂时停止响应新请求之前的短时间内多少个请求可以被存在堆栈中。 如果系统在一个短时间内有很多连接，则需要增大该参数的值，该参数值指定到来的TCP/IP连接的侦听队列的大小。不同的操作系统在这个队列大小上有它自己的限制。 试图设定back_log高于你的操作系统的限制将是无效的。默认值为50。对于Linux系统推荐设置为小于512的整数。
    key_buffer_size = 32M
    # key_buffer_size这对MyISAM表来说非常重要。如果只是使用MyISAM表，可以把它设置为可用内存的 30-40%。合理的值取决于索引大小、数据量以及负载 -- 记住，MyISAM表会使用操作系统的缓存来缓存数据，因此需要留出部分内存给它们，很多情况下数据比索引大多了。尽管如此，需要总是检查是否所有的 key_buffer 都被利用了 -- .MYI 文件只有 1GB，而 key_buffer 却设置为 4GB 的情况是非常少的。这么做太浪费了。如果你很少使用MyISAM表，那么也保留低于 16-32MB 的key_buffer_size 以适应给予磁盘的临时表索引所需。

    innodb_buffer_pool_size = 2.4G (服务器内存*0.7或0.8，得出的值)
    #这对Innodb表来说非常重要。Innodb相比MyISAM表对缓冲更为敏感。MyISAM可以在默认的 key_buffer_size 设置下运行的可以，然而Innodb在默认的innodb_buffer_pool_size 设置下却跟蜗牛似的。由于Innodb把数据和索引都缓存起来，无需留给操作系统太多的内存，因此如果只需要用Innodb的话则可以设置它高达 70-80% 的可用内存。-- 如果你的数据量不大，并且不会暴增，那么无需把innodb_buffer_pool_size 设置的太大了。

    innodb_additional_pool_size = 20M
    #这个选项对性能影响并不太多，至少在有差不多足够内存可分配的操作系统上是这样。不过如果你仍然想设置为 20MB(或者更大)，因此就需要看一下Innodb其他需要分配的内存有多少。

    innodb_log_file_size = 512M
    #在高写入负载尤其是大数据集的情况下很重要。这个值越大则性能相对越高，但是要注意到可能会增加恢复时间。我经常设置为64-512MB，根据服务器大小而异。

    innodb_log_buffer_size =16M
    #默认的设置在中等强度写入负载以及较短事务的情况下，服务器性能还可以。如果存在更新操作峰值或者负载较大，就应该考虑加大它的值了。如果它的值设置太高了，可能会浪费内存 -- 它每秒都会刷新一次，因此无需设置超过1秒所需的内存空间。通常8-16MB就足够了。越小的系统它的值越小。
    innodb_flush_log_at_trx_commit = 2
    #是否为Innodb比MyISAM慢1000倍而头大?看来也许你忘了修改这个参数了。默认值是 1，这意味着每次提交的更新事务(或者每个事务之外的语句)都会刷新到磁盘中，而这相当耗费资源，尤其是没有电池备用缓存时。很多应用程序，尤其是从 MyISAM转变过来的那些，把它的值设置为 2 就可以了，也就是不把日志刷新到磁盘上，而只刷新到操作系统的缓存上。日志仍然会每秒刷新到磁盘中去，因此通常不会丢失每秒1-2次更新的消耗。如果设置为0就快很多了，不过也相对不安全了 -- MySQL服务器崩溃时就会丢失一些事务。设置为2指挥丢失刷新到操作系统缓存的那部分事务。
    max_allowed_packet = 4M
    thread_stack = 256K
    table_cache = 128K
    sort_buffer_size = 6M    (32GB内存，48M) (64GB内存，96M)
    #查询排序时所能使用的缓冲区大小。注意：该参数对应的分配内存是每连接独占！如果有100个连接，那么实际分配的总共排序缓冲区大小为100 × 6 ＝ 600MB。所以，对于内存在4GB左右的服务器推荐设置为6-8M。
    read_buffer_size = 4M   (32GB内存，32M) (64GB内存，64M)
    #读查询操作所能使用的缓冲区大小。和sort_buffer_size一样，该参数对应的分配内存也是每连接独享！
    join_buffer_size = 8M   (32GB内存，64M) (64GB内存，128M)
    #联合查询操作所能使用的缓冲区大小，和sort_buffer_size一样，该参数对应的分配内存也是每连接独享！
    myisam_sort_buffer_size = 64M
    table_cache = 512
    #打开一个表的开销可能很大。例如MyISAM把MYI文件头标志该表正在使用中。你肯定不希望这种操作太频繁，所以通常要加大缓存数量，使得足以最大限度地缓存打开的表。它需要用到操作系统的资源以及内存，对当前的硬件配置来说当然不是什么问题了。如果你有200多个表的话，那么设置为 1024 也许比较合适(每个线程都需要打开表)，如果连接数比较大那么就加大它的值。我曾经见过设置为100,000的情况。
    thread_cache_size = 64
    #线程的创建和销毁的开销可能很大，因为每个线程的连接/断开都需要。我通常至少设置为 16。如果应用程序中有大量的跳跃并发连接并且 Threads_Created 的值也比较大，那么我就会加大它的值。它的目的是在通常的操作中无需创建新线程。
    query_cache_size = 64M

    #指定MySQL查询缓冲区的大小。可以通过在MySQL控制台执行以下命令观察：
    # > SHOW VARIABLES LIKE '%query_cache%';
    # > SHOW STATUS LIKE 'Qcache%';
    # 如果Qcache_lowmem_prunes的值非常大，则表明经常出现缓冲不够的情况；如果Qcache_hits的值非常大，则表明查询缓冲使用非常频繁，如果该值较小反而会影响效率，那么可以考虑不用查询缓冲；Qcache_free_blocks，如果该值非常大，则表明缓冲区中碎片很多。

    tmp_table_size = 256M
    max_connections = 768
    #指定MySQL允许的最大连接进程数。如果在访问论坛时经常出现Too Many Connections的错误提 示，则需要增大该参数值。
    max_connect_errors = 10000000
    wait_timeout = 10
    #指定一个请求的最大连接时间，对于4GB左右内存的服务器可以设置为5-10。
    thread_concurrency = 8  （2芯16核32线程，64）
    #该参数取值为服务器逻辑CPU数量×2，在本例中，服务器有2颗物理CPU，而每颗物理CPU又支持H.T超线程，所以实际取值为4 × 2 ＝ 8
    #skip-networking
    #开启该选项可以彻底关闭MySQL的TCP/IP连接方式，如果WEB服务器是以远程连接的方式访问MySQL数据库服务器则不要开启该选项！否则将无法正常连接！
 
注：配置好后，需要进入mysql 目录（/var/lib/mysql），删除原来的日志文件，才能成功重启mysql服务

`rm -rf ib_logfile*`


如果默认想用innodb的话，添加（修改为）以下内容：

    default-storage-engine=innodb
    default-table-type=innodb (在mysql 5.5.33中会导致无法启动，需注释掉)
    character-set-server=utf8
    collation-server=utf8_general_ci