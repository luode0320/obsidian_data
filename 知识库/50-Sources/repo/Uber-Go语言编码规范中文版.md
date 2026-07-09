---
id: 20260709-000000-uber-go-guide-cn
type: source
title: Uber Go 语言编码规范中文版（来源摘要）
aliases:
  - uber_go_guide_cn
  - Uber Go Style Guide 中文版
tags:
  - go
  - style-guide
  - source-repo
status: active
created: 2026-07-09
updated: 2026-07-09
source_sessions: []
source_refs:
  - https://github.com/xxjwxc/uber_go_guide_cn
  - https://github.com/uber-go/guide
related:
  - [[20-Knowledge/go/Go语言大厂编码规范资料清单|Go语言大厂编码规范资料清单]]
  - [[20-Knowledge/go/golang-patterns合并候选清单|golang-patterns合并候选清单]]
entities:
  - golang-patterns
  - luode-skills
topics:
  - Go 编码规范
confidence: high
---

# Uber Go 语言编码规范中文版（来源摘要）

## 来源

通过 curl 拉取 https://raw.githubusercontent.com/xxjwxc/uber_go_guide_cn/master/README.md 真实读取全文（3900 行），本笔记为结构化摘要，不是逐字复制。原文对应 uber-go/guide 官方英文版的社区中文翻译。

## 摘要（按原文目录结构）

### 指导原则

- 指向 interface 的指针：几乎不需要，应传值，底层数据仍可是指针；对象要修改底层数据必须用指针实现接口。
- Interface 合理性验证：用 var _ SomeInterface = (*Impl)(nil) 在编译期检查类型是否实现了接口，把运行期报错前移到编译期。
- 接收器与接口：值接收者方法集是指针接收者方法集的子集；同一类型的方法集要保持接收者风格一致，否则值对象无法满足要求指针接收者方法的接口。
- 零值 Mutex 是有效的：sync.Mutex 和 sync.RWMutex 零值可直接用，不需要 new(sync.Mutex)；且 mutex 应作为结构体非指针字段，不应作为匿名嵌入字段（会把 Lock/Unlock 意外导出）。
- 在边界处拷贝 Slices 和 Maps：接收和返回 slice/map 时要注意，它们内部是指针，直接持有引用会让调用方能修改内部状态，必要时用 make+copy 或逐个复制。
- 使用 defer 释放资源：Lock/Unlock、文件 Close 等应尽早 defer，避免多个 return 分支遗漏释放；defer 开销极小，可读性收益远大于成本。
- Channel 的 size 要么是 1，要么是无缓冲的：其它容量需要经过严格评审，说明白高负载下的阻塞和竞态语义。
- 枚举从 1 开始：用 iota+1 让默认零值 0 表示未设置/无效状态，除非零值本身就是有意义的默认行为。
- 使用 time 处理时间：瞬时时刻用 time.Time，时间段用 time.Duration；跨系统交互（JSON/SQL/YAML/命令行）优先用 time 包原生支持的编解码，无法使用时才退化为 int/float64+单位后缀命名。

### Errors

- 错误类型选择矩阵：不需要匹配+静态文案用 errors.New；不需要匹配+动态文案用 fmt.Errorf；需要匹配+静态文案用顶层 var err = errors.New()；需要匹配+动态文案用自定义 error 类型。
- 错误包装：无新增上下文就原样返回；需要上下文用 fmt.Errorf 的 %w（调用方需要可匹配底层错误）或 %v（不希望调用方依赖底层错误）；上下文文案要简洁，避免逐层堆叠 failed to。
- 错误命名：导出的哨兵错误变量用 Err 前缀（ErrNotFound），未导出用 err 前缀（errNotFound）；自定义错误类型用 Error 后缀（NotFoundError）。
- 一次处理错误：调用方只应对一个错误做一次处理（记录 或 包装返回 或 匹配后降级），不要先记录日志再原样返回，否则上层会重复处理造成日志噪音。
- 处理断言失败：类型断言必须用 v, ok := x.(T) 的双返回值形式，禁止单返回值形式（会在类型不符时 panic）。
- 不要使用 panic：生产代码必须返回 error 而不是 panic；只有不可恢复的情况（如启动期模板解析失败）才允许 panic；测试代码里用 t.Fatal 而不是 panic。

### 全局状态与生命周期

- 使用 go.uber.org/atomic：Uber 自家的原子类型库，为 int32/int64/bool 等提供类型安全封装，避免直接操作 sync/atomic 原语时忘记走原子路径；本仓库若不想引入 uber 系依赖，可只吸收其思想（原子字段要有明确的原子操作入口，不裸用 int32 加注释 atomic）。
- 避免可变全局变量：用依赖注入（构造函数注入 func 字段）替代包级 var 函数指针，便于测试替换。
- 避免在公共结构中嵌入类型：嵌入会泄漏实现细节、限制类型演化（加/删嵌入类型的方法都是破坏性变更）；应该用具名字段+手写委托方法。
- 避免使用内置名称：不要用 error、string、len、cap 等预声明标识符做变量名/字段名，会造成遮蔽和可读性混淆。
- 避免使用 init()：init 应该是完全确定的、不依赖其它 init 顺序、不做 IO 和环境访问；不满足则应放进 main() 或显式的初始化函数。
- 主函数退出方式：os.Exit/log.Fatal 只允许在 main() 里调用一次；其它函数一律返回 error，通过一个 run() 函数收拢所有失败路径，保证 defer 清理不会被跳过。
- 序列化结构使用字段标记：任何要序列化到 JSON/YAML 的结构体字段都应该显式写 tag，把序列化契约和字段重命名解耦。
- 不要一劳永逸地使用 goroutine：每个 goroutine 必须有可预测的停止时机或可发信号停止的机制，并且有代码能阻塞等待它退出（WaitGroup 或 done channel）；不要在 init() 里启动 goroutine，应该由一个显式的 Worker 类型管理生命周期（提供 Shutdown/Close 方法）。

### 性能

- 优先 strconv 而不是 fmt：数字转字符串场景 strconv.Itoa 比 fmt.Sprint 更快、分配更少。
- 避免字符串到字节的重复转换：固定字符串只转一次 []byte，缓存结果复用。
- 指定容器容量：make 时带上 len/cap 提示；slice 容量提示是硬分配（可避免后续 append 扩容拷贝），map 容量只是估算提示（仍可能分配，但能减少 rehash）。

### 规范（代码格式与写法）

- 避免过长的行：建议软限制 99 字符，非硬性。
- 一致性优先：同一 package 级别的写法应该统一，不一致的代价大于风格选择本身的对错。
- 相似声明放一组：import/const/var/type 各自分组书写，不相关的声明不要混进同一组；但函数内部相邻声明的变量即使不相关也可以分组书写。
- import 分组：标准库和第三方库分两组，goimports 默认行为。
- 包名：全小写、不用下划线、不用复数、不要叫 common/util/shared/lib 这类无信息量的名字。
- 函数名：MixedCaps，测试函数允许用下划线分组（TestFoo_EdgeCase）。
- 导入别名：只在包名和路径最后一段不一致，或者确实冲突时才用别名，否则不要滥用别名。
- 函数分组与顺序：导出函数在前，按接收者分组，NewXYZ 紧跟类型定义之后，纯工具函数放文件末尾。
- 减少嵌套：优先处理错误/特殊情况并尽早 return/continue，保持正常路径在最低缩进层级。
- 不必要的 else：if 两个分支都是给同一变量赋值时，改写成先赋默认值再单个 if 覆盖。
- 顶层变量声明：不显式写类型，除非表达式类型和期望类型不一致。
- 未导出顶层常量/变量用 _ 前缀（_defaultPort），例外是未导出错误变量用 err 前缀而不叠加下划线。
- 结构体中的嵌入：嵌入字段放字段列表最顶部并用空行隔开常规字段；嵌入必须提供实质好处，不能只为了省委托代码、不能改变外部类型零值可用性、不能让内部实现细节泄漏到公共 API。
- 本地变量声明：明确赋值用 :=；需要零值语义时用 var（比如声明 nil slice）。
- nil 是有效的 slice：判空用 len(s)==0 而不是 s==nil；不需要显式返回空 slice 字面量，直接 return nil；var 声明的 nil slice 可以直接 append，无需先 make。
- 缩小变量作用域：能收进 if 语句的赋值就收进去（if err := f(); err != nil），除非后续还要用到该变量。
- 避免参数语义不明确：布尔裸参数用行内注释标注含义，或者直接改成具名的枚举类型参数。
- 使用原始字符串字面值：需要转义的字符串优先用反引号包裹的 raw string。
- 初始化结构体：几乎总是要写字段名（不用位置字面量）；零值字段除非上下文需要说明否则省略；全零值结构体用 var 声明而不是 T{}；初始化引用类型用 &T{} 而不是 new(T)。
- 初始化 Maps：空 map 用 make()，让声明和赋值语义区分开；固定元素集合用 map literal 直接初始化。
- 字符串 format：函数外声明的 Printf 格式串应该是 const，方便 go vet 静态检查。
- 命名 Printf 样式函数：自定义的 xxxf 函数要以 f 结尾（如 Wrapf），配合 go vet -printfuncs 检查。

### 编程模式

- 表驱动测试：重复的测试逻辑用 []struct{give, want}{} 切片+t.Run 子测试；测试切片变量习惯命名 tests，单条用 tt，字段用 give/want 前缀；并行测试要在循环内重新声明 tt := tt 避免闭包捕获同一份循环变量。
- 功能选项模式：构造函数参数超过 3 个或需要可选扩展时用 Option 接口+WithXxx 构造器，而不是把所有参数堆进签名。

### Linting

推荐 errcheck、goimports、golint、govet、staticcheck，Runner 推荐 golangci-lint。

## 引用限制

本笔记为学习/工程决策用途的结构化摘要，保留了原文的规则要点和最小示意，未整篇复制原始 Markdown 全文；如需完整原文请访问 source_refs 中的仓库地址。原文遵循其仓库自身开源许可。
