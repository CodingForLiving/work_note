## 域名解析 getaddrinfo gethostbyname
* /etc/nsswitch.conf
  - 配置示例 hosts: files dns
  - files 表示先查询 /etc/hosts
  - dns 表示通过 DNS 查询，DNS 查询依赖于 /etc/resolv.conf 文件中配置的 DNS 服务器
* /etc/hosts
* /etc/resolv.conf
  - nameserver 8.8.8.8
* /etc/gai.conf 用于配置 getaddrinfo 的行为，例如地址排序规则
  - precedence ::ffff:0:0/96  100 表示优先ipv4(IPv4-mapped IPv6 address)
* 临时禁用ipv6
  - sysctl -w net.ipv6.conf.all.disable_ipv6=1
  - sysctl -w net.ipv6.conf.default.disable_ipv6=1
* 永久禁用ipv6
  - /etc/sysctl.conf
  - sysctl -w net.ipv6.conf.all.disable_ipv6=1
  - sysctl -w net.ipv6.conf.default.disable_ipv6=
