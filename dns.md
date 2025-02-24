# 域名解析 getaddrinfo gethostbyname

## 查询命令 nslookup [选项] [域名] [DNS服务器]
 - -type=A：查询A记录（IPv4地址）。
 - -type=AAAA：查询AAAA记录（IPv6地址）。
 - -type=MX：查询邮件交换记录。
 - -type=NS：查询域名服务器记录。
 - -type=CNAME：查询别名记录。
 - -type=TXT：查询文本记录。
 - -debug：显示详细的调试发信息
 - -port=端口号：指定DNS服务器端口（默认53）
 - -timeout=秒数：设置查询超时秒数
 - -retry=次数：设置重试次数

## 查询过程
* /etc/nsswitch.conf
  - 配置示例 hosts: files dns
  - files 表示先查询 /etc/hosts
  - dns 表示通过 DNS 查询，DNS 查询依赖于 /etc/resolv.conf 文件中配置的 DNS 服务器
* /etc/hosts
* /etc/resolv.conf
  - nameserver 8.8.8.8
* /etc/gai.conf 用于配置 getaddrinfo 的行为，例如地址排序规则
  - precedence ::ffff:0:0/96  100 表示优先ipv4(IPv4-mapped IPv6 address)

## 常见问题
* dns服务器无法访问
* dns服务器内容不全，新增的域名信息更新不及时
* dns服务器不响应AAAA查询,默认getaddrinfo会查询A和AAAA两种信息，如果查不到AAAA(ipv6)会超时
* 防火墙丢弃dns AAAA查询（封禁了ipv6 dns解析）解决办法：ai_family = AF_INET，强制程序只使用ipv4


