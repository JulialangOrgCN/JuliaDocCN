# JuliaDocCN
Julia官文中文（简体）译本（**不止**）。

**It is never too late to start right now.**

## 约定

- 符号
  - （） - 旁白或原文词句注释
  - 【】 - 强调
  - **深沉** - 成竹在胸
  - *内涵* - 胸无成竹
- 词汇
  - JulialangOrgCN - [朱华社区](http://julialang.org.cn/)
  - Julia - 茱莉亚（雅|娅）、祝丽、猪吏、主力
  - 群众 - 眸子雪亮的人、码农、大家、用户
  - 栗子 - 例子、梨子、黎姿
- 插图
  - Windows 10上Julia REPL
- 资源
 - [完整演示代码](https://github.com/JulialangOrgCN/howtojulia "HowtoJulia")

---

热烈欢迎客官审阅Julia 1.0的官文（中国大陆普通文字翻译）。

请阅读【[发布通告](https://julialang.org/blog/2018/08/one-point-zero)】获知语言概述和自Julia 0.6以来的变更。

要知道Juia 0.7主要是给Julia 1.0发布之前的代码提供包和代码提供升级路径（就略显尴尬地愣在那儿）。

因为Julia 0.7和Julia 1.0唯一不同就是后者去掉了废弃（deprecated）警告。

欲知Julia 0.6至Julia 1.0的完整变更列表，看【[Julia 0.7发布说明](https://docs.julialang.org/en/v0.7.0/NEWS/)】。

---

# 目录

- [前言](./前言.md "Introduction")
- [开发环境](./开发环境.md "DevEnv")

## 手册

- [上手](./手册/上手.md "Getting Started")
- [变量](./手册/变量.md "Variables")
- [整型数字和浮点型数字](./手册/整型数字和浮点型数字.md "Integers and Floating-Point Numbers")
- [算术操作符和基本函数](./手册/算术操作和基本函数.md "Mathematical Operations and Elementary Functions")
- [复数和有理数](./手册/复数和有理数.md "Complex and Rational Numbers")
- [字符串](./手册/字符串.md "Strings")
- [函数](./手册/函数.md "Functions")
- [控制流](./手册/控制流.md "Control Flow")
- [变量的作用域](./手册/变量的作用域.md "Scope of Variables")
- [类型](./手册/类型.md "Types")
- 方法
- 构造函数
- 转换（Conversion）和提升（Promotion）
- 接口
- 模块
- 文档
- 元编程
- 多维数组
- 值缺失
- 网络和流
- 并行计算
- 运行外部程序
- 调用C和Fortran代码
- 处理操作系统变种
- 环境变量
- 嵌入Julia
- 代码加载
- 一探究竟（Profiling）
- 栈回溯
- 性能窍门
- 工作流技巧
- 风格指南
- 常见问题（FAQ）
- 亮点（Noteworthy Differences from other Languages）
- 统一字符编码（Unicode）输入

## 基础

- 精要（Essentials）
- 集合和数据结构
- 算术
- 数字
- 字符串
- 数组
- 任务
- 多线程
- 常量
- 文件系统
- 输入输出（IO）和网络
- 标点符号（Punctuation）
- 排序及相关函数
- 迭代实用工具
- C接口
- C标准库
- 栈回溯
- 单指令多数据（SIMD）支持

## 标准库

- Base64
- CRC32c
- Dates
- Delimited Files ?
- 分布式计算
- 文件事件
- 交互式实用工具
- LibGit2
- 动态链接
- 线性代数
- 日志
- Markdown
- 内存映射输入输出（Memory-mapped IO）
- 包（Pkg）
- Printf
- 匍匐（Profiling）
- Julia的交互式解释器（REPL: Read-Eval-Print-Loop）
- 随机数字
- SHA
- 序列化
- 共享数组
- Sockets
- 稀疏（Sparse）数组
- 统计
- 单元测试
- UUID
- 统一字符编码（Unicode）

## 开发者文档

- 反射和内省（Introspection）
- Julia内部构件公文
  - Julia运行时初始化
  - Julia抽象语法树（AST: Abstract Syntax Trees）
  - 类型剖析
  - Julia对象的内存布局
  - Julia代码运算（Eval: 重新运算求出参数的内容）
  - 调用约定
  - 本地代码过程的高级概述
  - Julia函数
  - Base.Cartesian（笛卡尔）
  - 对话编译器【:meta机制】
  - 子数组
  - 位联合优化（isbits）
  - 系统镜像构建
  - 结合LLVM
  - Julia运行时的printf()和stdio
  - 边界检测
  - 警惕（Proper maintenance and care of）多线程锁
  - 客户定制索引的数组
  - 模块加载
  - 推断（Inference）
- 开发调试Julia的C代码
  - 报告并分析崩溃【段错误（segfault）】
  - GDB调试秘诀
  - 结合Valgrind
  - 错误检测（Sanitizer）支持

## 未归档的

  - [构建Windows版本](./开发者文档/README.windows.md)
