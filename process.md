## 所有进程

### ps
 - ps -aux 显示所有进程
 - ps -ef 完整格式列出所有进程
 - ps -u username 显示指定用户进程

### top
 - top -H 显示线程
 - 1 显示cpu核
 - M 按内存排序

### pgrep skynet -la 显示指定程序的所有进程

### pidof

##单个进程

### /proc/pid/status

### lsof -p PID

### strace -f -p PID

### ltrace -p PID 跟踪库函数调用

### gdb

### gcore

### pstack

### perf 性能分析工具

### ipcs 显示进程间通信状态

### fuser 显示使用文件或套接字的进程

### procrank 显示进程内存使用情况


