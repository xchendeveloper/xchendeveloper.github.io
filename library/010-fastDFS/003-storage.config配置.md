# storage.config
<br>

>1. [disabled](#disabled "disabled")
1. [group_name](#group_name "group_name")
1. [bind_addr](#bind_addr "bind_addr")
1. [client_bind](#client_bind "client_bind")
1. [port](#port "port")
1. [connect_timeout](#connect_timeout "connect_timeout")
1. [network_timeout](#network_timeout "network_timeout")
1. [heart_beat_interval](#heart_beat_interval "heart_beat_interval")
1. [stat_report_interval](#stat_report_interval "stat_report_interval")
1. [base_path](#base_path "base_path")
1. [max_connections](#max_connections "max_connections")
1. [work_threads](#work_threads "work_threads")
1. [buff_size](#buff_size "buff_size")
1. [disk_rw_direct](#disk_rw_direct "disk_rw_direct")
1. [disk_rw_separated](#disk_rw_separated "disk_rw_separated")
1. [disk_reader_threads](#disk_reader_threads "disk_reader_threads")
1. [disk_writer_threads](#disk_writer_threads "disk_writer_threads")
1. [sync_wait_msec](#sync_wait_msec "sync_wait_msec")
1. [sync_interval](#sync_interval "sync_interval")
1. [sync_start_time & sync_end_time](#sync_start_time & sync_end_time "sync_start_time & sync_end_time")
1. [write_mark_file_freq](#write_mark_file_freq "write_mark_file_freq")
1. [store_path_count](#store_path_count "store_path_count")
1. [store_path0](#store_path0 "store_path0")
1. [subdir_count_per_path](#subdir_count_per_path "subdir_count_per_path")
1. [tracker_server](#tracker_server "tracker_server")
1. [log_level](#log_level "log_level")
1. [run_by_group](#run_by_group "run_by_group")
1. [run_by_user](#run_by_user "run_by_user")
1. [allow_hosts](#allow_hosts "allow_hosts")
1. [file_distribute_path_mode](#file_distribute_path_mode "file_distribute_path_mode")
1. [file_distribute_rotate_count](#file_distribute_rotate_count "file_distribute_rotate_count")
1. [fsync_after_written_bytes](#fsync_after_written_bytes "fsync_after_written_bytes")
1. [sync_log_buff_interval](#sync_log_buff_interval "sync_log_buff_interval")
1. [sync_binlog_buff_interval](#sync_binlog_buff_interval "sync_binlog_buff_interval")
1. [sync_stat_file_interval](#sync_stat_file_interval "sync_stat_file_interval")
1. [thread_stack_size](#thread_stack_size "thread_stack_size")
1. [upload_priority](#upload_priority "upload_priority")
1. [check_file_duplicate](#check_file_duplicate "check_file_duplicate")
1. [file_signature_method](#file_signature_method "file_signature_method")
1. [key_namespace](#key_namespace "key_namespace")
1. [keep_alive](#keep_alive "keep_alive")
1. [fashDHT 配置](#fashDHT 配置 "fashDHT 配置")
1. [use_access_log](#use_access_log "use_access_log")
1. [rotate_access_log](#rotate_access_log "rotate_access_log")
1. [access_log_rotate_time](#access_log_rotate_time "access_log_rotate_time")
1. [rotate_error_log](#rotate_error_log "rotate_error_log")
1. [error_log_rotate_time](#error_log_rotate_time "error_log_rotate_time")
1. [rotate_access_log_size](#rotate_access_log_size "rotate_access_log_size")
1. [rotate_error_log_size](#rotate_error_log_size "rotate_error_log_size")
1. [file_sync_skip_invalid_record](#file_sync_skip_invalid_record "file_sync_skip_invalid_record")
1. [http.disabled & http.server_port & http.trunk_size & http.domain_name](#http.disabled & http.server_port & http.trunk_size & http.domain_name "http.disabled & http.server_port & http.trunk_size & http.domain_name")


## disabled
disabled=false，这个配置文件是否不生效

## group_name
group_name=group1 ，指定 此 storage server 所在 组(卷)

## bind_addr
bind_addr= ，后面为绑定的IP地址 (常用于服务器有多个IP但只希望一个IP提供服务)。如果不填则表示所有的(一般不填就OK)

## client_bind
client_bind=true ，bind_addr通常是针对server的。当指定bind_addr时，本参数才有效。<br>
本storage server作为client连接其他服务器（如tracker server、其他storage server），是否绑定bind_addr。

## port
port=23000 ，storage server服务端口

## connect_timeout
connect_timeout=30 ，连接超时时间，针对socket套接字函数connect

## network_timeout
network_timeout=60 ，tracker server的网络超时，单位为秒。发送或接收数据时，如果在超时时间后还不能发送或接收数据，则本次网络通信失败。

## heart_beat_interval
heart_beat_interval=30 ，心跳间隔时间，单位为秒 (这里是指主动向tracker server 发送心跳)

## stat_report_interval
stat_report_interval=60 ，storage server向tracker server报告磁盘剩余空间的时间间隔，单位为秒。

## base_path
base_path=/home/yuqing/fastdfs ，base_path 目录地址,根目录必须存在  子目录会自动生成 (注 :这里不是上传的文件存放的地址,之前是的,在某个版本后更改了)

## max_connections
max_connections=256 ，系统提供服务时的最大连接数

## work_threads
work_threads=4 ,工作线程数，通常设置为CPU数

## buff_size
buff_size = 256KB ，设置队列结点的buffer大小。工作队列消耗的内存大小 = buff_size * max_connections，设置得大一些，系统整体性能会有所提升。
消耗的内存请不要超过系统物理内存大小。另外，对于32位系统，请注意使用到的内存不要超过3GB

## disk_rw_direct
disk_rw_direct = false ，设置为true，表示不使用操作系统的文件内容缓冲特性。如果文件数量很多，且访问很分散，可以考虑将本参数设置为true

## disk_rw_separated
disk_rw_separated = true ，磁盘IO读写是否分离，缺省是分离的。

## disk_reader_threads
disk_reader_threads = 1 ，针对单个存储路径的读线程数，缺省值为1。<br>
读写分离时，系统中的读线程数 = disk_reader_threads * store_path_count<br>
读写混合时，系统中的读写线程数 = (disk_reader_threads + disk_writer_threads) * store_path_count

## disk_writer_threads
disk_writer_threads = 1 ，针对单个存储路径的写线程数，缺省值为1。<br>
读写分离时，系统中的写线程数 = disk_writer_threads * store_path_count <br>
读写混合时，系统中的读写线程数 = (disk_reader_threads + disk_writer_threads) * store_path_count

## sync_wait_msec
sync_wait_msec=200 ，同步文件时，如果从binlog中没有读到要同步的文件，休眠N毫秒后重新读取。0表示不休眠，立即再次尝试读取。<br>
出于CPU消耗考虑，不建议设置为0。如何希望同步尽可能快一些，可以将本参数设置得小一些，比如设置为10ms

## sync_interval
sync_interval=0 ,同步上一个文件后，再同步下一个文件的时间间隔，单位为毫秒，0表示不休眠，直接同步下一个文件。

## sync_start_time & sync_end_time
sync_start_time=00:00 ，sync_end_time=23:59 ,允许系统同步的时间段 (默认是全天) 。一般用于避免高峰同步产生一些问题而设定。

## write_mark_file_freq
write_mark_file_freq=500 ，同步完N个文件后，把storage的mark文件同步到磁盘，注：如果mark文件内容没有变化，则不会同步

## store_path_count
store_path_count=1 ，存放文件时storage server支持多个路径（例如磁盘）。这里配置存放文件的基路径数目，通常只配一个目录。

## store_path0
store_path0=/home/yuqing/fastdfs<br>
store_path1=/home/yuqing/fastdfs2<br>
逐一配置store_path个路径，索引号基于0。注意配置方法后面有0,1,2 ......，需要配置0到store_path - 1。<br>
如果不配置base_path0，那边它就和base_path对应的路径一样。

## subdir_count_per_path
subdir_count_per_path=256 ，FastDFS存储文件时，采用了两级目录。这里配置存放文件的目录个数 (系统的存储机制,大家看看文件存储的目录就知道了)，<br>
如果本参数只为N（如：256），那么storage server在初次运行时，会自动创建 N * N 个存放文件的子目录。

## tracker_server
tracker_server=10.62.164.84:22122<br>
tracker_server=10.62.245.170:22122<br>
tracker_server 的列表 要写端口的哦，有多个tracker server时，每个tracker server写一行

## log_level
log_level=info<br>
- emerg for emergency
- alert
- crit for critical
- error
- warn for warning
- notice
- info
- debug

## run_by_group
run_by_group= ， ，操作系统运行FastDFS的用户组 (不填 就是当前用户组,哪个启动进程就是哪个)

## run_by_user
run_by_user= ，操作系统运行FastDFS的用户 (不填 就是当前用户,哪个启动进程就是哪个)

## allow_hosts
allow_hosts=* ，可以连接到此 storage server 的ip范围（对所有类型的连接都有影响，包括客户端，storage server）
```
 "*" means match all ip addresses, can use range like this: 10.0.1.[1-15,20] or
 host[01-08,20-25].domain.com, for example:
 allow_hosts=10.0.1.[1-15,20]
 allow_hosts=host[01-08,20-25].domain.com
```

## file_distribute_path_mode
file_distribute_path_mode=0 ,文件在data目录下分散存储策略。

- 0: 轮流存放，在一个目录下存储设置的文件数后（参数file_distribute_rotate_count中设置文件数），使用下一个目录进行存储。
- 1: 随机存储，根据文件名对应的hash code来分散存储。

## file_distribute_rotate_count
file_distribute_rotate_count=100 ，当上面的参数file_distribute_path_mode配置为0（轮流存放方式）时，本参数有效。<br>
当一个目录下的文件存放的文件数达到本参数值时，后续上传的文件存储到下一个目录中。

## fsync_after_written_bytes
fsync_after_written_bytes=0 ，当写入大文件时，每写入N个字节，调用一次系统函数fsync将内容强行同步到硬盘。0表示从不调用fsync 。

## sync_log_buff_interval
sync_log_buff_interval=10 ，同步或刷新日志信息到硬盘的时间间隔，单位为秒。注意：storage server 的日志信息不是时时写硬盘的，而是先写内存。

## sync_binlog_buff_interval
sync_binlog_buff_interval=60 ，同步binglog（更新操作日志）到硬盘的时间间隔，单位为秒。本参数会影响新上传文件同步延迟时间。


## sync_stat_file_interval
sync_stat_file_interval=300 ，把storage的stat文件同步到磁盘的时间间隔，单位为秒。注：如果stat文件内容没有变化，不会进行同步

## thread_stack_size
thread_stack_size=512KB ，线程栈的大小。FastDFS server端采用了线程方式。<br>
对于V1.x，storage server线程栈不应小于512KB；对于V2.0，线程栈大于等于128KB即可。<br>
线程栈越大，一个线程占用的系统资源就越多。<br>
对于V1.x，如果要启动更多的线程（max_connections），可以适当降低本参数值。

## upload_priority
upload_priority=10 ，本storage server作为源服务器，上传文件的优先级，可以为负数。值越小，优先级越高。这里就和 tracker.conf 中store_server= 2时的配置相对应了。

## check_file_duplicate
check_file_duplicate=0 ，是否检测上传文件已经存在。如果已经存在，则不存在文件内容，建立一个符号链接以节省磁盘空间。
这个应用要配合FastDHT 使用，所以打开前要先安装FastDHT，1或yes 是检测，0或no 是不检测

## file_signature_method
file_signature_method=hash , 文件去重时，文件内容的签名方式：
- hash： 4个hash code
- md5：MD5

## key_namespace
key_namespace=FastDFS ，当上个参数设定为1 或 yes时 (true/on也是可以的) ， 在FastDHT中的命名空间。

## keep_alive
keep_alive=0 ，与FastDHT servers 的连接方式 (是否为持久连接) ，默认是0（短连接方式）。可以考虑使用长连接，这要看FastDHT server的连接数是否够用。

## fashDHT 配置
可以通过 #include filename 方式来加载 FastDHT servers  的配置，装上FastDHT就知道该如何配置啦。check_file_duplicate=1 时才有用，不然系统会忽略
```
include /home/yuqing/fastdht/conf/fdht_servers.conf
```

## use_access_log
use_access_log = false ，是否将文件操作记录到access log。

## rotate_access_log
rotate_access_log = false , 是否定期轮转access log，目前仅支持一天轮转一次

## access_log_rotate_time
access_log_rotate_time=00:00 ，access log定期轮转的时间点，只有当rotate_access_log设置为true时有效

## rotate_error_log
rotate_error_log = false ,是否定期轮转error log，目前仅支持一天轮转一次

## error_log_rotate_time
error_log_rotate_time=00:00 ，error log定期轮转的时间点，只有当rotate_error_log设置为true时有效

## rotate_access_log_size
rotate_access_log_size = 0 , access log按文件大小轮转, 设置为0表示不按文件大小轮转，否则当access log达到该大小，就会轮转到新文件中

## rotate_error_log_size
rotate_error_log_size = 0 ，error log按文件大小轮转，设置为0表示不按文件大小轮转，否则当error log达到该大小，就会轮转到新文件中

## file_sync_skip_invalid_record
file_sync_skip_invalid_record=false ,文件同步的时候，是否忽略无效的binlog记录

## http.disabled & http.server_port & http.trunk_size & http.domain_name
http.disabled=false <br>
http.server_port=8888 <br>
http.trunk_size=256KB http.trunk_size表示读取文件内容的buffer大小（一次读取的文件内容大小），也就是回复给HTTP client的块大小。<br>
http.domain_name=,storage server上web server域名，通常仅针对单独部署的web server。这样URL中就可以通过域名方式来访问storage server上的文件了，这个参数为空就是IP地址的方式。 <br>
use "#include" directive to include HTTP other settiongs
```
include http.conf
```
