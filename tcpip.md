### tcp连接建立过程
 - 我方发送SYN包
 - 正常返回syn-ack
 - 我发返回ack
 异常情况
 - 我方发送syn
 - 对方返回RST包（端口未开放)

### netcat命令(用于连通性测试）
 - 监听tcp nc -l -p 5000
 - 连接tcp nc 127.0.0.1 5000
 - 监听upd nc -l -u -p 5000
 - 连接udp -u 127.0.0.1 5000
 - nc -l -s 192.168.1.100 -p 1234
 - -p表示本地端口
 - -s表示本地地址

### ping命令
 - PING 192.168.1.1 (192.168.1.1): 56 data bytes
 - 64 bytes from 192.168.1.1: icmp_seq=0 ttl=64 time=1.234 ms
 - 64 bytes：表示 ICMP 报文的大小（包括头部和数据）。
 - icmp_seq：表示 ICMP 报文的序列号，用于标识报文的顺序。
 - ttl：表示报文的生存时间（Time to Live），每经过一个路由器会减 1，用于防止报文无限循环。
 - time：表示往返时间（RTT），即从发送 Echo Request 到收到 Echo Reply 的时间。
 - 如何主机禁用了icmp，会ping不通，不代表主机不存在
 - 在某些网络中，ICMP 报文的优先级较低

### icmp用途
 - ping 
 - 超时（Time Exceeded）：traceroute 
 - 目标不可达（Destination Unreachable）：当路由器无法将数据包发送到目标主机时，会发送 ICMP 目标不可达消息。
 - 重定向（Redirect）

### netstat
 - netstat可能需要安装net-tools包
 - 所有连接：netstat -a
 - tcp连接：netstat -at
 - udp连接：netstat -au
 - 监听端口：netstat -l
 - 接口统计信息：netstat -i
 - 路由表：netstat -r
 - 进程id和进程名称：netstat -p
 - 持续显示信息：netstat -c
 - 以数字形式显示地址和端口: netstat -n

### ss命令（Socket Statistics）
 - 用于查看网络套接字统计信息,能够显示TCP、UDP、UNIX域套接字等详细信息
 - ss命令通常比netstat更快，因为它直接从内核获取信息，而不需要解析/proc/net文件
 - 显示所有连接: ss -a
 - -t tcp
 - -u udp
 - -l 监听
 - -n 数字格式
 - -o 计时器信息
 - ss -s 显示摘要信息
 - ss -t state established 过滤指定状态连接
   - established
   - syn-sent
   - syn-recv
   - fin-wait-1
   - fin-wait-2
   - time-wait
   - closed
   - close-wait
   - last-ack
   - listening
   - closing
 - 过滤源地址:  ss -tn src 192.168.1.100
 - 过滤目标地址: ss -tn dst 192.168.1.1
 - 过滤端口: ss -tn sport = :80
 - 过滤目标端口：ss -tn dport = ：443
