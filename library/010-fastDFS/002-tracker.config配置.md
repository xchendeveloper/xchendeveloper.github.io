# tracker.config 配置
<br>

>1. [disabled](#disabled "disabled")
1. [bind_addr](#bind_addr "bind_addr")
1. [port](#port "port")
1. [connect_timeout](#connect_timeout "connect_timeout")
1. [network_timeout](#network_timeout "network_timeout")
1. [base_path](#base_path "base_path")
1. [max_connections](#max_connections "max_connections")
1. [work_threads](#work_threads "work_threads")
1. [store_lookup](#store_lookup "store_lookup")
1. [store_group](#store_group "store_group")
1. [store_server](#store_server "store_server")
1. [store_path](#store_path "store_path")
1. [download_server](#download_server "download_server")
1. [reserved_storage_space](#reserved_storage_space "reserved_storage_space")
1. [log_level](#log_level "log_level")
1. [run_by_group](#run_by_group "run_by_group")
1. [run_by_user](#run_by_user "run_by_user")
1. [allow_hosts](#allow_hosts "allow_hosts")
1. [sync_log_buff_interval](#sync_log_buff_interval "sync_log_buff_interval")
1. [check_active_interval](#check_active_interval "check_active_interval")
1. [thread_stack_size](#thread_stack_size "thread_stack_size")
1. [storage_ip_changed_auto_adjust](#storage_ip_changed_auto_adjust "storage_ip_changed_auto_adjust")
1. [storage_sync_file_max_delay](#storage_sync_file_max_delay "storage_sync_file_max_delay")
1. [storage_sync_file_max_time](#storage_sync_file_max_time "storage_sync_file_max_time")
1. [use_trunk_file](#use_trunk_file "use_trunk_file")
1. [slot_min_size](#slot_min_size "slot_min_size")
1. [slot_max_size](#slot_max_size "slot_max_size")
1. [trunk_file_size](#trunk_file_size "trunk_file_size")
1. [trunk_create_file_advance](#trunk_create_file_advance "trunk_create_file_advance")
1. [trunk_create_file_time_base](#trunk_create_file_time_base "trunk_create_file_time_base")
1. [trunk_create_file_interval](#trunk_create_file_interval "trunk_create_file_interval")
1. [trunk_create_file_space_threshold](#trunk_create_file_space_threshold "trunk_create_file_space_threshold")
1. [trunk_init_check_occupying](#trunk_init_check_occupying "trunk_init_check_occupying")
1. [trunk_init_reload_from_binlog](#trunk_init_reload_from_binlog "trunk_init_reload_from_binlog")
1. [use_storage_id](#use_storage_id "use_storage_id")
1. [storage_ids_filename](#storage_ids_filename "storage_ids_filename")
1. [store_slave_file_use_link](#store_slave_file_use_link "store_slave_file_use_link")
1. [rotate_error_log](#rotate_error_log "rotate_error_log")
1. [error_log_rotate_time](#error_log_rotate_time "error_log_rotate_time")
1. [rotate_error_log_size](#rotate_error_log_size "rotate_error_log_size")
1. [http.disabled & http.server_port](#http.disabled & http.server_port "http.disabled & http.server_port")



## disabled
disabled=false，这个配置文件是否不生效

## bind_addr
bind_addr= ，后面为绑定的IP地址 (常用于服务器有多个IP但只希望一个IP提供服务)。如果不填则表示所有的(一般不填就OK)

## port
port=22122 ，提供服务的端口

## connect_timeout
connect_timeout=30 ，连接超时时间，针对socket套接字函数connect

## network_timeout
network_timeout=60 ，tracker server的网络超时，单位为秒。发送或接收数据时，如果在超时时间后还不能发送或接收数据，则本次网络通信失败。

## base_path
base_path=/home/yuqing/fastdfs ，base_path 目录地址(根目录必须存在,子目录会自动创建)
附目录说明:
```
  tracker server目录及文件结构：
  ${base_path}
    |__data
    |     |__storage_groups.dat：存储分组信息
    |     |__storage_servers.dat：存储服务器列表
    |__logs
          |__trackerd.log：tracker server日志文件

  数据文件storage_groups.dat和storage_servers.dat中的记录之间以换行符（\n）分隔，字段之间以西文逗号（,）分隔。
  storage_groups.dat中的字段依次为：
    1. group_name：组名
    2. storage_port：storage server端口号

  storage_servers.dat中记录storage server相关信息，字段依次为：
    1. group_name：所属组名
    2. ip_addr：ip地址
    3. status：状态
    4. sync_src_ip_addr：向该storage server同步已有数据文件的源服务器
    5. sync_until_timestamp：同步已有数据文件的截至时间（UNIX时间戳）
    6. stat.total_upload_count：上传文件次数
    7. stat.success_upload_count：成功上传文件次数
    8. stat.total_set_meta_count：更改meta data次数
    9. stat.success_set_meta_count：成功更改meta data次数
    10. stat.total_delete_count：删除文件次数
    11. stat.success_delete_count：成功删除文件次数
    12. stat.total_download_count：下载文件次数
    13. stat.success_download_count：成功下载文件次数
    14. stat.total_get_meta_count：获取meta data次数
    15. stat.success_get_meta_count：成功获取meta data次数
    16. stat.last_source_update：最近一次源头更新时间（更新操作来自客户端）
    17. stat.last_sync_update：最近一次同步更新时间（更新操作来自其他storage server的同步）
```

## max_connections
max_connections=256 ，系统提供服务时的最大连接数

## work_threads
work_threads=4 ,工作线程数，通常设置为CPU数

## store_lookup
store_lookup=2 ，上传组(卷) 的方式 0:轮询方式 1: 指定组 2: 平衡负载(选择最大剩余空间的组(卷)上传)。
这里如果在应用层指定了上传到一个固定组,那么这个参数被绕过

## store_group
store_group=group2 ，当上一个参数设定为1 时 (store_lookup=1，即指定组名时)，必须设置本参数为系统中存在的一个组名。如果选择其他的上传方式，这个参数就没有效了。

## store_server
store_server=0 , 选择哪个storage server 进行上传操作(一个文件被上传后，这个storage server就相当于这个文件的storage server源，会对同组的storage server推送这个文件达到同步效果)

- 0: 轮询方式
- 1: 根据ip 地址进行排序选择第一个服务器（IP地址最小者）
- 2: 根据优先级进行排序（上传优先级由storage server来设置，参数名为upload_priority）

## store_path
store_path=0 ，中的哪个目录进行上传。storage server可以有多个存放文件的base path（可以理解为多个磁盘）。

- 0: 轮流方式，多个目录依次存放文件
- 2: 选择剩余空间最大的目录存放文件（注意：剩余磁盘空间是动态的，因此存储到的目录或磁盘可能也是变化的）

## download_server
download_server=0 ，选择哪个 storage server 作为下载服务器
- 0: 轮询方式，可以下载当前文件的任一storage server
- 1: 哪个为源storage server 就用哪一个 (前面说过了这个storage server源 是怎样产生的) 就是之前上传到哪个storage server服务器就是哪个了

## reserved_storage_space
reserved_storage_space = 10% ,storage server 上保留的空间，保证系统或其他应用需求空间。可以用绝对值或者百分比（V4开始支持百分比方式）。
(指出 如果同组的服务器的硬盘大小一样,以最小的为准,也就是只要同组中有一台服务器达到这个标准了,这个标准就生效,原因就是因为他们进行备份)

## log_level
log_level=info ，选择日志级别(日志写在哪?看前面的说明了,有目录介绍哦)

## run_by_group
run_by_group= ，操作系统运行FastDFS的用户组 (不填 就是当前用户组,哪个启动进程就是哪个)

## run_by_user
run_by_user= ，操作系统运行FastDFS的用户 (不填 就是当前用户,哪个启动进程就是哪个)

## allow_hosts
allow_hosts=* ，可以连接到此 tracker server 的ip范围（对所有类型的连接都有影响，包括客户端，storage server）
```
 "*" means match all ip addresses, can use range like this: 10.0.1.[1-15,20] or
 host[01-08,20-25].domain.com, for example:
 allow_hosts=10.0.1.[1-15,20]
 allow_hosts=host[01-08,20-25].domain.com
```

## sync_log_buff_interval
sync_log_buff_interval = 10 ，同步或刷新日志信息到硬盘的时间间隔，单位为秒，注意：tracker server 的日志不是时时写硬盘的，而是先写内存。

## check_active_interval
check_active_interval = 120 ，检测 storage server 存活的时间隔，单位为秒。

storage server定期向tracker server 发心跳，如果tracker server在一个check_active_interval内还没有收到storage server的一次心跳，那边将认为该storage server已经下线。所以本参数值必须大于storage server配置的心跳时间间隔。通常配置为storage server心跳时间间隔的2倍或3倍。

## thread_stack_size
thread_stack_size=1MB ，线程栈的大小。FastDFS server端采用了线程方式。更正一下，tracker server线程栈不应小于64KB。线程栈越大，一个线程占用的系统资源就越多。如果要启动更多的线程

## storage_ip_changed_auto_adjust
storage_ip_changed_auto_adjust=true ，这个参数控制当storage server IP地址改变时，集群是否自动调整。注：只有在storage server进程重启时才完成自动调整。

## storage_sync_file_max_delay
storage_sync_file_max_delay = 86400 ，存储服务器之间同步文件的最大延迟时间，缺省为1天。根据实际情况进行调整。
注：本参数并不影响文件同步过程。本参数仅在下载文件时，判断文件是否已经被同步完成的一个阀值（经验值）

## storage_sync_file_max_time
storage_sync_file_max_time = 300 ，存储服务器同步一个文件需要消耗的最大时间，缺省为300s，即5分钟。
注：本参数并不影响文件同步过程。本参数仅在下载文件时，作为判断当前文件是否被同步完成的一个阀值（经验值）

## use_trunk_file
use_trunk_file = false ，是否使用小文件合并存储特性，缺省是关闭的。

## slot_min_size
slot_min_size = 256 ，trunk file分配的最小字节数。比如文件只有16个字节，系统也会分配slot_min_size个字节。

## slot_max_size
slot_max_size = 16MB ，只有文件大小<=这个参数值的文件，才会合并存储。如果一个文件的大小大于这个参数值，将直接保存到一个文件中（即不采用合并存储方式）。

## trunk_file_size
trunk_file_size = 64MB ，合并存储的trunk file大小，至少4MB，缺省值是64MB。不建议设置得过大。

## trunk_create_file_advance
trunk_create_file_advance = false ，是否提前创建trunk file。只有当这个参数为true，下面3个以trunk_create_file_打头的参数才有效。

## trunk_create_file_time_base
trunk_create_file_time_base = 02:00 ，提前创建trunk file的起始时间点（基准时间），02:00表示第一次创建的时间点是凌晨2点。

## trunk_create_file_interval
trunk_create_file_interval = 86400 ，创建trunk file的时间间隔，单位为秒。如果每天只提前创建一次，则设置为86400

## trunk_create_file_space_threshold
trunk_create_file_space_threshold = 20G ，提前创建trunk file时，需要达到的空闲trunk大小，比如本参数为20G，而当前空闲trunk为4GB，那么只需要创建16GB的trunk file即可。

## trunk_init_check_occupying
trunk_init_check_occupying = false ，trunk初始化时，是否检查可用空间是否被占用。

## trunk_init_reload_from_binlog
是否无条件从trunk binlog中加载trunk可用空间信息
FastDFS缺省是从快照文件storage_trunk.dat中加载trunk可用空间，
该文件的第一行记录的是trunk binlog的offset，然后从binlog的offset开始加载

## use_storage_id
use_storage_id = false ，是否使用server ID作为storage server标识

## storage_ids_filename
storage_ids_filename = storage_ids.conf ，use_storage_id 设置为true，才需要设置本参数。
在文件中设置组名、server ID和对应的IP地址，参见源码目录下的配置示例：conf/storage_ids.conf

## store_slave_file_use_link
store_slave_file_use_link = false ，存储从文件是否采用symbol link（符号链接）方式。
如果设置为true，一个从文件将占用两个文件：原始文件及指向它的符号链接。

## rotate_error_log
rotate_error_log = false ，是否定期轮转error log，目前仅支持一天轮转一次。

## error_log_rotate_time
error_log_rotate_time=00:00 ，error log定期轮转的时间点，只有当rotate_error_log设置为true时有效

## rotate_error_log_size
rotate_error_log_size = 0 ，error log按大小轮转。
设置为0表示不按文件大小轮转，否则当error log达到该大小，就会轮转到新文件中

## http.disabled & http.server_port
http.disabled=false ，HTTP服务是否不生效<br>
http.server_port=8080 ， HTTP服务端口<br>
关于http的设置，默认编译是不生效的 要求更改 #WITH_HTTPD=1 将 注释#去掉 再编译<br>
关于http的应用 说实话 不是很了解 没有见到 相关说明<br>
use "#include" directive to include http other settiongs<br>
include http.conf 如果加载http.conf的配置文件
