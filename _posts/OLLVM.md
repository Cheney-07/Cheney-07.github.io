基于LLVM的开源项目，LLVM是一个完整的编译器架构。
LLVM的核心思想是将编译器分为前端和后端两个部分，前端负责将源代码转换为中间表示LLVM Intermediate Representation （IR）（以库的形式提供接口），后端负责将中间表示转换为目标机器的汇编代码。![[Pasted image 20251203202627.png]]
## LLVM IR

三种表示：

- .ll 格式：人类可以阅读的文本。
- .bc 格式：适合机器存储的二进制文件。
- 内存表示
## LLVM Pass

> [https://llvm.org/docs/WritingAnLLVMPass.html#the-immutablepass-class](https://llvm.org/docs/WritingAnLLVMPass.html#the-immutablepass-class)

Pass就是“遍历一遍IR，可以同时对它做一些操作”的意思，Pass 是 中间 IR 处理环节的主要模块。  
llvm pass的作用：

- 插桩
- 优化机器无关的代码

### ModulePass (基于模块)

如果函数传递不需要任何模块或不可变传递，则模块传递可以使用接口的函数级传递（例如支配者）来提供检索分析结果的函数。

### FunctionPass (基于函数)

### CallGraphPass (基于调用图)

### LoopPass (基于循环)
### PASS 注册方式

1、修改 PassRegistry.def 和 PassBuilder.cpp 文件，直接追加 Pass 定义进去。  
2、是用插件接口进行注册

### LLVM Pass 实现「指令替代混淆」"LLVM Pass 实现「指令替代混淆」

|步骤|操作|
|---|---|
|注册|通过 `llvmGetPassPluginInfo()` 注册|
|遍历|遍历 `Function` / `BasicBlock`|
|修改|进行 IR 控制流改造（添加跳转 / switch / 伪逻辑）|

---
# OLLVM
## 虚假控制流

原理：
- 虚假控制流混淆通过加入包含不透明谓词的条件跳转（永真or永假）和不可达的基本块，来干扰 IDA 的控制流分析和 F5 反汇编。

---
反混淆：  
1、将全局变量赋值并将 segment 设为只读。

- 对于常规的 ollvm 的 bcf 混淆来说，bcf 的不透明谓词都是处于 .bss段 中。
- Edit->Segments->Edit segment 将 Write 复选框取消勾选， .bss段 就设为只读
2、d810
3、idapython patch 不透明谓词

---
## 指令替换（Instruction Substitution）

原理：

- 用于将程序中的原始指令替换为等效但更难理解和还原的指令序列，增加逆向难度
---
反混淆：  
1、d810  
2、GAMBA

---
## 控制流平坦化（FLA）

原理：

- 通过一个主分发器来控制程序基本块的执行流程


![[Pasted image 20251203203500.png]]
- 序言：函数的第一个执行的基本块
- 主 (子) 分发器：控制程序跳转到下一个待执行的基本块
- retn 块：函数出口
- 真实块：混淆前的基本块，程序真正执行工作的块
- 预处理器：跳转到主分发器

各块之间的规则：

1. 函数的开始地址为序言的地址
2. 序言的后继为主分发器
3. 后继为主分发器的块为预处理器
4. 后继为预处理器的块为真实块
5. 无后继的块为retn块
6. 剩下的为无用块
---
反混淆：  
1、**找到真实块**。手撕；idapython 通过各个块之间的联系通过一定的规则找真实块；可以用 unicorn 或 angr 得到函数的 CFG, 利用规则匹配出真实块…  
![[Pasted image 20251203204352.png]]

方法多种多样，但是核心都是找到真实块，除真实块和序言块外，其余的块都是虚假块，我们需要 NOP 掉他们。  
2、**得到真实块之间的联系**。模拟执行pr真机调试打断点 trace。  
3、用跳转汇编指令将每个真实块**串**起来。

也可以使用工具（deflat-master和llvm-fla-cracker-master）
![[Pasted image 20251203205132.png]]
