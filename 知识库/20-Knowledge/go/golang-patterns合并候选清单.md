---
id: 20260709-000000-golang-patterns-merge-candidates
type: knowledge
title: golang-patterns合并候选清单（Uber Go规范）
aliases:
  - golang-patterns合并计划
  - Uber规范吸收决策
tags:
  - go
  - golang-patterns
  - skill-evolution
status: active
created: 2026-07-09
updated: 2026-07-09
source_sessions: []
source_refs:
  - 知识库/50-Sources/repo/Uber-Go语言编码规范中文版.md
  - golang-patterns/SKILL.md
related:
  - [[20-Knowledge/go/Go语言大厂编码规范资料清单|Go语言大厂编码规范资料清单]]
  - [[50-Sources/repo/Uber-Go语言编码规范中文版|Uber-Go语言编码规范中文版]]
entities:
  - golang-patterns
  - luode-skills
topics:
  - Go 编码规范
  - skill 内容演进
confidence: high
---

# golang-patterns合并候选清单（Uber Go规范）

## 定义

对比 Uber Go 语言编码规范中文版 与本仓库现有 golang-patterns/SKILL.md，逐条判定：直接采纳、需要与现状调和、明确冲突不采纳、第三方依赖不采纳。这是实施计划的决策依据，不是最终实施文档本身。

## 已被现有规则覆盖，不重复吸收

错误必须显式处理、errors.Is/As、Functional Options、依赖注入替代包级可变状态、goroutine防泄漏+panic保护、组合优于继承——golang-patterns现状已有对应条目，Uber版本可作为补充例证但不构成新增条目。

## 明确冲突，需要显式记录决策，不能静默合并

- 冲突点：Uber规范的枚举从1开始一节推荐用 iota+1；本仓库golang-patterns现有常量与枚举一节明确禁止用iota定义常量和枚举，理由是跨包跨模块复用时容易因插入顺序发生语义漂移。
- 决策建议：保留本仓库现有铁律，不采纳iota写法；但吸收其背后原理——默认零值应代表未设置或无效状态，改写成不依赖iota的等价表达（显式赋值时让第一个有效枚举值从1开始，0留给零值默认，写明注释原因）。
- 该条目需要在SKILL.md里显式写一句这是刻意与Uber规范不同的地方及原因，避免以后又被其它来源规范覆盖回iota。

## 第三方专属依赖，不采纳为强制规则

- 使用 go.uber.org/atomic：Uber自家原子类型库，不作为本仓库强制依赖；只吸收思想，即原子字段要有清晰的原子操作入口，不要裸用int32配注释atomic。
- go.uber.org/goleak：goroutine泄漏测试库，同样不强制引入，仅作为可选测试工具提示放进工具链章节的推荐清单，不写成必须。

## 采纳候选分组（供实施周期拆分参考）

### 分组A：接口与并发安全（约9条）

指向interface的指针不需要、Interface合理性验证（var _ X = (*Impl)(nil)编译期检查）、接收器与接口的值/指针方法集规则（用于把现有反模式条目值接收者与指针接收者混用升级为正式规则）、零值Mutex有效且不作匿名嵌入、在边界处拷贝Slices和Maps、使用defer释放资源、Channel的size要么1要么无缓冲、等待goroutines退出的WaitGroup/done channel两种写法、不要在init()中启动goroutine改用显式Worker生命周期管理。

### 分组B：错误处理体系细化（约6条）

错误类型选择矩阵（是否需要匹配x是否为动态文案的四种组合）、错误包装%w与%v的选择原则及避免failed to堆叠、错误命名Err/err前缀与Error后缀、一次处理错误原则、处理断言失败必须用comma-ok、不要panic的生产代码与测试代码两种表述强化。

### 分组C：代码格式与初始化规范（约17条，内容量大）

避免过长的行、相似声明放一组、import分组、包名规范、减少嵌套与不必要的else的反例强化、顶层变量声明不显式写类型、未导出顶层常量变量_前缀、本地变量声明:=优先与缩小变量作用域、nil是有效的slice、避免参数语义不明确、初始化结构体四条规则、初始化Maps、避免使用内置名称、避免在公共结构中嵌入类型、结构体中嵌入的位置与例外规则、避免使用init()的四条判断标准、主函数退出方式Exit的run模式。

### 分组D：性能与测试补充（约7条）

优先strconv而不是fmt、避免字符串到字节的重复转换、指定Map容量提示、追加时优先指定切片容量（与现有预分配slice合并）、序列化结构使用字段标记、命名Printf样式函数以f结尾、表驱动测试写法（tests/tt/give-want命名约定与并行测试tt:=tt陷阱，这是全新章节，现状完全没有测试写法指导）。

时间处理单独一条：使用time.Time和time.Duration而不是裸int表示时间，可归入分组B或独立成条。

## 落点方案比选

- 方案A：全部扩充进 golang-patterns/SKILL.md 正文。预计从239行增至约480到520行，改动集中，一次读完不用跳转；但会顶到implementation-review-rules里500行需拆分评估的信号线。
- 方案B：SKILL.md保留现有精简结构，新增 golang-patterns/references/idioms-from-uber-guide.md 承载分组A到D的详细规则和代码示例，SKILL.md正文只加简短入口指引和少量最核心原则。符合本仓库其它大型skill的SKILL.md精简加references承载细节的既有架构惯例。
- 推荐方案B，理由：与仓库既有大型skill结构一致，且规避单文件行数超限信号；SKILL.md继续保持可以整篇读完的核心原则清单定位。

## 关联

需求文档与实施总览见 doc/2-需求 与 doc/3-实施 下对应文件（由implementation-planning-rules落地时创建），本笔记只承载决策依据，不重复维护实施步骤本身。
