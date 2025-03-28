# NASM (Netwide Assembler) 详细介绍

NASM (Netwide Assembler) 是一个开源的x86汇编器，支持多种目标文件格式，被广泛用于操作系统开发、逆向工程和性能关键型代码编写。

## NASM 概述

- **全称**: Netwide Assembler
- **类型**: 汇编器 (Assembler)
- **平台**: 跨平台 (Windows, Linux, macOS等)
- **许可**: BSD风格的开源许可
- **特点**:
  - 支持多种x86架构 (16位, 32位, 64位)
  - 语法简洁直观
  - 支持宏汇编
  - 可生成多种目标文件格式
  - 活跃的开源社区支持

## NASM 支持的目标文件格式

NASM 可以生成多种目标文件格式，包括但不限于：
- `bin`: 纯二进制文件 (无格式)
- `aout`: Linux a.out 格式
- `coff`: COFF 格式 (用于Windows)
- `elf`: ELF 格式 (Linux, Unix-like)
- `macho`: Mach-O 格式 (macOS)
- `win32`/`win64`: PE 格式 (Windows)
- `obj`: Microsoft OMF 格式

## NASM 基本语法结构

```nasm
section .data           ; 数据段
    msg db 'Hello, World!', 0xA

section .text           ; 代码段
    global _start       ; 入口点声明

_start:
    mov eax, 4          ; sys_write 系统调用
    mov ebx, 1          ; 文件描述符 (stdout)
    mov ecx, msg        ; 消息地址
    mov edx, 13         ; 消息长度
    int 0x80            ; 调用内核
    
    mov eax, 1          ; sys_exit 系统调用
    int 0x80            ; 调用内核
```

## NASM 支持的指令集

### 1. 数据传输指令

- `MOV`: 数据传送
  ```nasm
  mov eax, 10       ; 立即数到寄存器
  mov [var], eax    ; 寄存器到内存
  mov eax, [var]    ; 内存到寄存器
  ```

- `XCHG`: 交换数据
  ```nasm
  xchg eax, ebx     ; 交换eax和ebx的值
  ```

- `LEA`: 加载有效地址
  ```nasm
  lea eax, [ebx+ecx*4] ; eax = ebx + ecx*4
  ```

### 2. 算术运算指令

- `ADD`, `SUB`: 加减法
  ```nasm
  add eax, 10       ; eax += 10
  sub ebx, ecx      ; ebx -= ecx
  ```

- `INC`, `DEC`: 自增/自减
  ```nasm
  inc eax           ; eax++
  dec ebx           ; ebx--
  ```

- `MUL`, `IMUL`: 无符号/有符号乘法
  ```nasm
  mul ebx           ; edx:eax = eax * ebx (无符号)
  imul eax, ebx     ; eax = eax * ebx (有符号)
  ```

- `DIV`, `IDIV`: 无符号/有符号除法
  ```nasm
  div ebx           ; eax = edx:eax / ebx, edx = 余数
  ```

### 3. 逻辑运算指令

- `AND`, `OR`, `XOR`, `NOT`: 逻辑运算
  ```nasm
  and eax, 0xFF     ; eax &= 0xFF
  or ebx, 0x80      ; ebx |= 0x80
  xor eax, eax      ; eax = 0 (快速清零)
  not ecx           ; ecx = ~ecx
  ```

- `SHL`, `SHR`: 逻辑移位
  ```nasm
  shl eax, 2        ; eax <<= 2
  shr ebx, 1        ; ebx >>= 1
  ```

- `SAL`, `SAR`: 算术移位
  ```nasm
  sal eax, 1        ; 算术左移 (同SHL)
  sar ebx, 3        ; 算术右移 (保留符号位)
  ```

### 4. 控制转移指令

- `JMP`: 无条件跳转
  ```nasm
  jmp label         ; 跳转到label
  ```

- 条件跳转:
  ```nasm
  je label          ; 等于时跳转 (ZF=1)
  jne label         ; 不等于时跳转 (ZF=0)
  jg label          ; 有符号大于时跳转
  jl label          ; 有符号小于时跳转
  ja label          ; 无符号大于时跳转
  jb label          ; 无符号小于时跳转
  ```

- `CALL`, `RET`: 子程序调用和返回
  ```nasm
  call subroutine   ; 调用子程序
  ret               ; 从子程序返回
  ```

### 5. 栈操作指令

- `PUSH`, `POP`: 栈操作
  ```nasm
  push eax          ; 将eax压栈
  pop ebx           ; 弹出栈顶到ebx
  ```

- `PUSHF`, `POPF`: 标志寄存器操作
  ```nasm
  pushf             ; 将EFLAGS压栈
  popf              ; 从栈恢复EFLAGS
  ```

### 6. 字符串操作指令

- `MOVSB`, `MOVSW`, `MOVSD`: 字符串传送
  ```nasm
  mov esi, source   ; 源地址
  mov edi, dest     ; 目标地址
  mov ecx, len      ; 长度
  rep movsb         ; 按字节复制
  ```

- `CMPSB`, `CMPSW`, `CMPSD`: 字符串比较
- `SCASB`, `SCASW`, `SCASD`: 字符串扫描
- `STOSB`, `STOSW`, `STOSD`: 字符串存储
- `LODSB`, `LODSW`, `LODSD`: 字符串加载

### 7. 标志位操作指令

- `STC`, `CLC`, `CMC`: 设置/清除/取反进位标志
- `STD`, `CLD`: 设置/清除方向标志
- `STI`, `CLI`: 设置/清除中断标志

### 8. 系统相关指令

- `INT`: 软件中断
  ```nasm
  int 0x80          ; Linux系统调用
  ```

- `IRET`: 中断返回
- `SYSCALL`, `SYSRET`: 快速系统调用 (x86-64)
- `SYSENTER`, `SYSEXIT`: 快速系统调用 (x86)

### 9. 高级指令 (x86/x86-64)

- `CMOVcc`: 条件移动
  ```nasm
  cmovz eax, ebx    ; 如果ZF=1, 则eax=ebx
  ```

- `BSWAP`: 字节交换
  ```nasm
  bswap eax         ; 反转eax的字节顺序
  ```

- `CPUID`: CPU识别
- `RDTSC`: 读取时间戳计数器
- `XADD`, `CMPXCHG`: 原子操作指令
- `MMX`, `SSE`, `AVX` 等SIMD指令集

## NASM 特殊语法和特性

### 1. 宏系统

NASM 提供了强大的宏系统:

```nasm
%macro print 2       ; 定义带2个参数的宏
    mov eax, 4
    mov ebx, 1
    mov ecx, %1
    mov edx, %2
    int 0x80
%endmacro

; 使用宏
print msg, msg_len
```

### 2. 条件汇编

```nasm
%ifdef DEBUG
    ; 调试代码
%else
    ; 发布代码
%endif
```

### 3. 包含文件

```nasm
%include "io.inc"    ; 包含其他汇编文件
```

### 4. 结构体和记录

```nasm
struc person
    .name: resb 32
    .age:  resd 1
endstruc
```

## NASM 与 MASM/TASM 的区别

1. **语法差异**:
   - NASM 使用 `[ ]` 表示内存引用
   - MASM/TASM 在某些情况下不需要括号

2. **宏系统**:
   - NASM 的宏系统更加强大和灵活

3. **段处理**:
   - NASM 的段处理更加明确和一致

4. **跨平台**:
   - NASM 是跨平台的，而 MASM 主要是Windows平台

## 使用示例

### 简单的 Hello World (Linux)

```nasm
section .data
    msg db 'Hello, World!', 0xA
    len equ $ - msg

section .text
    global _start

_start:
    ; write(1, msg, len)
    mov eax, 4
    mov ebx, 1
    mov ecx, msg
    mov edx, len
    int 0x80

    ; exit(0)
    mov eax, 1
    xor ebx, ebx
    int 0x80
```

### 64位代码示例

```nasm
section .data
    msg db 'Hello, 64-bit World!', 0xA
    len equ $ - msg

section .text
    global _start

_start:
    ; write(1, msg, len)
    mov rax, 1          ; sys_write
    mov rdi, 1          ; stdout
    mov rsi, msg
    mov rdx, len
    syscall

    ; exit(0)
    mov rax, 60         ; sys_exit
    xor rdi, rdi
    syscall
```

## 总结

NASM 是一个功能强大且灵活的汇编器，支持从传统的16位代码到最新的x86-64指令集。它的语法清晰一致，宏系统强大，并且可以生成多种平台的目标文件格式。无论是学习汇编语言，还是开发底层系统软件，NASM 都是一个优秀的选择。
