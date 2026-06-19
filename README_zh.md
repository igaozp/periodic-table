# 计算机系统设计原则的元素周期表

#### [Joy Arulraj (佐治亚理工学院)](https://faculty.cc.gatech.edu/~jarulraj/)

系统设计通常通过特定领域的解决方案来教授，例如数据库、操作系统或计算机体系结构，每个领域都有自己的方法和术语。虽然这种多样性是一种优势，但它可能掩盖了跨领域反复出现的横切原则。本文提出了一个初步的分类法，从计算机系统的多个领域中提炼出系统设计原则。目标是建立一个共享、简洁的词汇表，帮助学生、研究人员和从业者对结构和权衡进行推理，跨领域比较设计，并更清晰地传达选择。

## 1. 引言

在计算机系统领域工作的一个回报是该领域的广泛多样性，涵盖操作系统、数据库、计算机体系结构、分布式系统、编程语言、网络等，每个领域都有丰富的历史。对于新手来说，由于传统和词汇的多样性，发现不同领域之间的联系可能具有挑战性：相同的设计原则可能以不同的形式出现在不同的领域。

例如，考虑 Jim Gray 等人关于数据库隔离级别的经典论文 [17]。它详细描述了并发控制机制以及正确性和性能之间的权衡。然而，如果没有接触过操作系统或计算机体系结构中的类似问题，这些想法可能看起来狭隘地"关于数据库"。实际上，相同的设计原则"一致性放松"以不同的形式在系统中重新出现——从弱有序的内存层次结构到分布式系统中的最终一致性协议。当每个社区使用自己的术语和范例时，新手可能很难识别底层设计原则。这种碎片化增加了认知开销，因为必须在每个上下文中重新学习相同的权衡。

这是一个更广泛的模式：系统研究在实践洞察方面丰富，但在共享概念框架方面较轻。跨领域，类似的挑战反复出现，管理并发性、确保一致性和适应变化，而框架和词汇通常不同。因此，看似不同的领域之间的深层联系可能相对模糊。

本文是弥合这些差距的一小步。借用门捷列夫的比喻，我们呈现了一个反复出现的系统设计原则的"元素周期表"。目标不是一个刚性的分类法，而是一个工作词汇表：一种简洁的方式来注释论文、讲座和设计文档中使用的基本原则。目标是揭示计算机系统中已经存在的结构，以便学生可以形成更连贯的心智地图，研究人员可以精确地定位贡献，从业者可以更清晰地讨论跨领域的设计选择。

## 2. 方法论

我们通过查看 100 多篇跨操作系统、计算机体系结构、数据库、网络、编程语言、安全和计算机系统其他领域的有影响力的论文来识别原则。这些论文因其历史意义和持续相关性而被选中，例如关于并发控制 [17] 和共识 [25] 的经典论文，以及关于在系统内部使用机器学习 [22] 和为云设计系统 [6] 的最新工作。

对于每篇论文，我们问：底层的高级设计原则是什么？跨领域，独立的系统通常不是在机制上收敛，而是在共享的设计原则上收敛：例如，放松一致性以提高性能或提升抽象以增强可用性。

要符合系统设计原则的资格，它必须满足两个条件：

1. **抽象** - 原则必须独立于特定的技术或实现。
2. **通用性** - 原则必须出现在不同的领域（例如，数据库系统、操作系统、编程语言）。

这种分析不旨在编目每个原则，而是要揭示许多具有持久、通用价值的原则。

## 3. 设计原则表

我们精心策划了一组 40 多个通用设计原则，从系统文献中提炼出来。如表 1 所示，它们被组织成主题组，反映了系统设计的熟悉轴。

每个原则都标有一个简短的符号（例如，`Co` 代表可组合性，`Op` 代表乐观设计）以便快速参考。我们强调**设计意图**而不是规定机制：不是"使用这种锁定协议"或"优化这个查询计划"，而是原则陈述目标，例如"在并发下保持正确性"或"优先考虑常见情况"，将具体实现留给特定领域。

## 目录

- [🟪 第 1 组：结构](#-第-1-组结构): *如何划分和连接具有清晰边界和扩展点的部分。*
- [🟧 第 2 组：效率](#-第-2-组效率): *通过将努力集中在有回报的地方，减少工作或更便宜地完成工作。*
- [🟨 第 3 组：语义](#-第-3-组语义): *精确指定行为和接口。*
- [⬛ 第 4 组：分布](#-第-4-组分布): *跨分布式架构协调工作和数据。*
- [🟩 第 5 组：规划](#-第-5-组规划): *从目标、成本和约束中自动选择计划。*
- [🟦 第 6 组：可操作性](#-第-6-组可操作性): *以最小的中断观察、适应和演化运行中的系统。*
- [🟥 第 7 组：可靠性](#-第-7-组可靠性): *在故障、并发和部分失败下保持正确。*
- [🟫 第 8 组：安全性](#-第-8-组安全性): *限制权限并强制隔离以保持安全和完整性。*

**图例：** `代码` = 唯一简短符号，`名称` = 原则，`意图` = 简短描述。

<img width="400" alt="periodic-table" src="https://github.com/user-attachments/assets/36e11f9b-e99b-4138-85c1-f92f1306c3d6" />


## 🟪 第 1 组：结构

🟪 **Si – 简单性**

选择满足当前需求的最简单的系统设计；抵制复杂性，例如"以防万一"添加的额外层、服务或通用性，直到证据显示有益处。

**示例：** 避免系统的过早架构优化 [23]。

🟪 **Mo – 模块化**

将系统划分为具有最小接口的内聚单元，以便每个单元可以独立地推理、替换或演化。这一原则侧重于分解：选择边界以支持清晰的关注点分离，以便每个责任都在一个模块中。

**示例：** OSI 模型将通信分解为具有明确定义的边界的标准化层，允许独立开发和替换 [48]。

🟪 **Co – 可组合性**

设计可以安全灵活地重新组合的组件；依赖显式合约和类型约束接口，以便每个合法组合保持正确，让组件像可互换的积木一样组装。与模块化不同，这一原则侧重于重新组合：确保组件可以安全灵活地组合。

**示例：** Unix 程序（例如，grep、sort、uniq）从 stdin 读取并写入 stdout，让用户组成复杂的文本处理管道 [41]。

🟪 **Ex – 可扩展性**

设计系统以允许安全的用户定义扩展，例如插件，而无需更改系统核心。当扩展来自不受信任的方时，通过沙盒隔离它们以保持安全。

**示例：** Unix 也说明了可扩展性：用户可以添加新程序而无需更改内核 [41]。

🟪 **Pm – 策略/机制分离**

通过公开一个通用接口，将应该做什么（策略）与如何执行（机制）分开，多个策略可以插入相同的机制。

**示例：** Hydra 有一个通用机制的内核（调度、分页、保护），并将资源分配策略移至用户级模块 [32]。

🟪 **Gr – 泛化设计**

设计一个具有显式变化点（如类型、旋钮或插件）的单一核心，以便它可以服务于许多用例而无需重复，但在这样做时可以在性能、准确性或清晰度方面获得有意义的收益时进行专业化。

**示例：** C++ 标准模板库是由模板参数化的容器、迭代器和算法的集合 [45]。Postgres 允许用户向核心数据库系统添加类型和操作符 [46]。

🟪 **Pd – 概率设计**

引入受控的随机性以获得效率、可扩展性或简单性，同时接受小的、量化的错误或损失风险。

**示例：** 路由器将队列长度视为概率信号：随着队列增长，它们以增加的概率丢弃传入的数据包，主动发出拥塞信号 [13]。

## 🟧 第 2 组：效率

🟧 **Sc – 可扩展性**

设计系统以处理数据、流量或节点的增长，成本或延迟接近线性。

**示例：** MapReduce 通过将工作划分为并行任务并以最小的协调聚合结果来跨节点扩展 [10]。

🟧 **Rc – 计算重用**

通过缓存、具体化中间结果（例如，索引）或跨重复或略微修改的输入增量更新输出来避免冗余工作，节省计算。

**示例：** B+ 树重用其排序的键顺序：查找遵循现有的搜索路径，而不是每次都重新扫描整个数据集，从而重用计算 [2]。

🟧 **Wv – 工作避免**

跳过不会改变外部可观察结果的计算。示例包括惰性求值和谓词短路。

**示例：** 惰性求值推迟工作，直到需要值，消除无用的计算 [19]。

🟧 **Cc – 常见情况专业化**

检测主导运行时的执行路径或数据项（"热点"），并为它们创建一个简化的快速路径，而较慢的通用路径仍然正确处理每种情况。

**示例：** 在第一次调用时缓存接收者类的目标方法，以便对该常见接收者的后续调用命中快速路径；不常见的类回退到完整的方法查找例程 [5]。

🟧 **Bo – 瓶颈导向优化**

分析端到端性能，定位最紧的资源约束，并将改进工作集中在那里，直到另一个阶段成为限制因素。

**示例：** 罕见的第 99 百分位掉队者限制延迟，复制请求有助于减少尾部响应时间 [9]。

🟧 **Ha – 硬件感知设计**

将算法和数据结构塑造为底层硬件的延迟、带宽、并行性和持久性属性（例如，缓存层次结构、NUMA、SSD、GPU）。

**示例：** BLAS 定义了缓存和向量调优的内核，因此线性代数代码可以有效地利用硬件 [31]。

🟧 **Op – 乐观设计**

假设常见情况会成功，跳过协调，并仅在该假设被证明错误时依赖（可能昂贵的）恢复路径。

**示例：** 乐观并发控制无锁运行事务，然后在提交时验证，并仅在检测到冲突时回滚 [24]。

🟧 **La – 学习近似**

用在数据上训练的模型替换手工制作的算法，以有限的不准确性换取效率或灵活性。

**示例：** 感知器分支预测器在线学习权重以预测分支结果，在不增大表的情况下优于固定的两位计数器 [22]。

## 🟨 第 3 组：语义

🟨 **Al – 抽象提升**

将低级操作包装在更高级别的接口或领域特定语言后面，该接口或语言表达意图而不是步骤。这使得内部优化成为可能，还允许单个定义针对不同的后端。

**示例：** SQL 查询声明要检索的结果；DBMS 自动选择访问路径、连接顺序和物理操作符 [44]。

🟨 **Lu – 语言同质性**

在核心组件和扩展中采用单一、定义明确的中间表示（或语言），以便语义对齐，工具组合，以及跨层优化和重用以最小的努力发生。

**示例：** LLVM 公开了一个基于类型的 SSA 的 IR，许多前端目标和许多后端共享，实现跨语言优化和重用相同的中间端传递 [30]。

🟨 **Se – 语义显式接口**

精确指定接口（涵盖效果可见性、排序、持久性等），以便用户可以推理调用的真实外部可观察状态，而无需猜测隐藏的缓冲或复制。

**示例：** SQL 隔离级别指定精确的异常语义并使可见性保证显式 [3]。

🟨 **Fs – 形式规范**

使用数学模型或逻辑描述系统行为，以支持严格的推理、验证或综合。实现这一原则的机制包括时态逻辑、状态机和其他使系统属性可分析的形式主义。

**示例：** TLA+ 展示了如何使用逻辑和集合论指定和检查系统，以在编码之前捕获设计错误 [27]。

🟨 **Ig – 不变量导向转换**

使用正式陈述的不变量来驱动安全的重构、优化或重新配置。

**示例：** 在编译器中，SSA 将"每个名称一个定义"视为 IR 不变量；传递重写代码，同时保持语义，然后重新建立 SSA [8]。在查询优化器中，关系代数等价（例如，选择/投影下推）保持结果语义 [44]。

## ⬛ 第 4 组：分布

⬛ **Lt – 位置透明性**

隐藏资源的物理位置，以便客户端通过统一的名称或句柄进行交互。

**示例：** 程序可以像调用本地过程一样调用远程过程，掩盖主机位置 [4]。

⬛ **Dc – 去中心化控制**

在许多节点之间分配决策，以避免单点故障或瓶颈。

**示例：** Dynamo 通过一致性哈希对数据进行分区，并使用基于 gossip 的成员资格，避免任何中央协调器 [12]。

⬛ **Fp – 功能放置**

将功能放置在存在必要上下文和资源的地方，以实现正确性和效率，避免在其他地方进行冗余工作。

**示例：** 端到端论证表明，像可靠性检查这样的功能仅在端点处实现正确性 [42]。

⬛ **Lo – 引用局部性**

将相关的数据和操作在时间和空间上放置在一起，以保持访问模式并最小化计算和状态之间的分离。

**示例：** 工作集模型形式化了时间局部性，以将热页保留在内存中 [11]。

⬛ **Cz – 协调避免**

设计计算和数据流以减少对分布式协调的需求，通过识别可以独立进行的操作，同时保持应用程序级别的正确性。

**示例：** CRDT 允许副本独立更新并确定性地合并状态，保证收敛而无需运行时协调 [47]。

## 🟩 第 5 组：规划

🟩 **Ep – 基于等价的规划**

对保持语义等价的通用 IR 应用代数/逻辑重写规则；将最终选择推迟到后续的成本/约束阶段。

**示例：** Starburst 的基于规则的重写系统应用关系等价（例如，谓词下推）以生成逻辑等价的查询 [39]。

🟩 **Cm – 基于成本的规划**

当系统必须在替代设计、配置或执行策略之间进行选择时，使用成本模型指导搜索朝向低成本解决方案（能源、金钱等），而无需枚举完整的空间。

**示例：** Selinger 查询优化器在成本模型下选择成本最低的计划 [44]。

🟩 **Cp – 基于约束的规划**

编码决策和硬或软约束，并依赖求解器（ILP/SMT 等）找到可行或最优的分配。

**示例：** Quincy 将集群调度表述为具有局部性和公平性约束的最小成本流，并求解它以获得分配 [21]。

🟩 **Gd – 目标导向规划**

接受所需最终状态的声明性描述，并自动合成达到该状态的具体操作序列，将用户从实现细节中屏蔽。

**示例：** Cascades 查询优化器通过基于规则的转换和成本导向的搜索将 SQL 查询（目标）转换为可执行计划 [14]。

🟩 **Bb – 黑盒调优**

当分析成本模型不可用时，通过在目标系统上测量候选项来搜索计划/配置空间，迭代地选择更好的候选项（例如，启发式或贝叶斯搜索），并缓存获胜者。

**示例：** ATLAS 在目标 CPU 上经验性地计时候选 BLAS 内核配置，并固定性能最佳的参数，而无需分析成本模型 [47]。

🟩 **Ah – 建议性提示**

提供非约束性提示，系统可以利用这些提示来提高性能，而不会改变正确性或需要强制执行。

**示例：** Lampson 提倡可选的"提示"，有助于性能，但如果被忽略则不得影响正确性 [29]。

## 🟦 第 6 组：可操作性

🟦 **Ad – 自适应处理**

监控运行时条件并自动调整参数或策略。

**示例：** Eddies 根据反馈在运行时持续重新排序查询操作符，在不停止执行的情况下进行适应 [1]。

🟦 **Ec – 弹性**

根据需求和成本目标的变化自动调整资源分配。示例包括预测性自动扩展和负载整形。

**示例：** Chase 等人根据负载和效用动态配置服务器，体现了弹性资源管理 [6]。

🟦 **Wa – 工作负载感知优化**

持续观察工作负载形状（倾斜、局部性、访问频率等），并调整数据布局、算法选择或资源分配以匹配当前模式。

**示例：** 数据库"cracking"根据查询谓词逐步重组列数据，持续将数据布局适应到观察到的工作负载 [20]。

🟦 **Au – 自动化和自主性**

让系统在没有人工干预的情况下执行例行或反应性任务，通常通过从跟踪或用户提供的示例中学习。

**示例：** AutoAdmin 从工作负载跟踪自动推荐索引/物化视图 [7]。通过示例编程的系统通过从几个用户提供的示例中进行泛化来自动化任务 [33]。

🟦 **Ho – 人类可观察性**

公开系统的内部状态，如指标、跟踪、计划，使系统有意透明；该透明度改善了可观察性、调试、内省和控制。

**示例：** Paxson 的端到端 Internet 数据包动态分析展示了丰富的测量和跟踪如何实现明智的调试和调优 [37]。

🟦 **Ev – 可演化性**

设计使系统可以以最小的停机时间或重写进行更改，并且在不破坏外部合约或现有客户端的可观察行为的情况下这样做。与可扩展性不同，可扩展性允许外部人员通过定义的钩子点添加新行为而不触及核心，可演化性允许系统的内部随时间变化而不破坏现有的外部合约。

**示例：** Parnas 展示了模块化设计如何使系统更容易扩展而无需破坏性重写 [36]。

## 🟥 第 7 组：可靠性

🟥 **Ft – 容错性**

设计系统以在组件故障的情况下继续运行，可能以降级的形式。

**示例：** Gray 对计算机停止原因的分析表明，复制和自动重启让服务通过硬件和软件故障继续运行 [15]。

🟥 **Is – 正确性隔离**

防止组件之间的意外干扰，以便本地推理保持有效。

**示例：** 两阶段行级锁定阻止一个事务读取或覆盖另一个事务的未提交数据，保持隔离保证 [16]。

🟥 **At – 原子执行**

将多个操作分组，使它们看起来不可分割，要么全部生效，要么都不生效。

**示例：** 使用事务内存，事务内的内存操作推测性地执行，然后原子提交；如果发生任何冲突或故障，整个块中止并且不留下部分状态 [18]。

🟥 **Cr – 一致性放松**

在文档化的边界内有意放松强一致性或排序约束，以提高性能、可用性或并发性。

**示例：** Bayou 允许移动客户端在断开连接时更新副本，在副本重新连接时保证最终收敛，以离线可用性交换严格一致性 [38]。

## 🟫 第 8 组：安全性

🟫 **Sy – 通过隔离的安全性**

强制执行强边界，以便故障或恶意代码不能影响其他组件。

**示例：** 正确的虚拟机监视器为每个客户机提供一个完整的、隔离的机器，并拦截特权操作，防止一个客户机危害其他客户机或主机 [40]。

🟫 **Ac – 访问控制和审计**

定义权限并记录每次访问以实现问责制。

**示例：** Lampson 的访问控制列表、能力和审计跟踪分类法支撑了现代安全机制 [28]。

🟫 **Lp – 最小权限**

仅授予任务所需的最小权限，缩小爆炸半径。

**示例：** 1988 年 Internet 蠕虫的事后分析显示，过度权限如何让蠕虫传播，并促使广泛采用最小权限守护进程 [35]。

🟫 **Tq – 通过法定人数的信任**

依赖多个独立参与者的协议，而不是单一权威。

**示例：** Paxos 算法在多数法定人数中复制状态，因此即使少数节点崩溃或恶意行为，服务也保持正确 [26]。

🟫 **Cf – 保守默认值**

附带限制性、安全的设置；让专家选择加入更冒险、更快的模式。

**示例：** 使用"默认无访问"策略，每个保护机制应仅在明确授予时允许访问 [43]。

🟫 **Sa – 构造安全性**

构造代码或数据，使整类错误变得不可能而不仅仅是被检测到。

**示例：** Rust 的所有权和借用检查器在编译时防止数据竞争和悬空指针 [34]。

## 4. 案例研究

为了说明多个设计原则如何在实践中交叉，考虑关系数据库系统中从逻辑到物理操作符计划的映射。

- 数据库系统将声明性意图转换为可执行步骤（**策略/机制分离**）。
- SQL 表达"什么"（**抽象提升**）具有精确的语义（**语义显式接口**）。
- 优化器首先使用代数等价重写查询（**基于等价的规划**）。
- 然后使用成本模型选择具体的物理操作符（**基于成本的规划**）。
- 物理操作符通常针对底层硬件特性进行优化（**硬件感知设计**）。
- 谓词下推说明了**工作避免**，而索引实现了**计算重用**。
- **建议性提示**可以指导优化器，较新的数据库系统添加运行时重新优化（**自适应处理**）、学习模型（**学习近似**）和采样（**概率设计**）。

因此，数据库系统中的逻辑到物理操作符映射例证了几个设计原则如何结合在一起以有效处理声明性 SQL 查询。

## 5. 局限性

任何试图组织像计算机系统这样广泛的领域的尝试都涉及权衡。这张表不是一个清单或一个普遍理论；它是一个共享的词汇表，突出了反复出现的原则并鼓励结构性反思。也就是说，存在几个局限性：

- **正交性**：原则可以重叠、加强或部分冲突；设计是关于平衡这些紧张关系。
- **主观性和粒度**：派生和映射原则涉及判断；边界是模糊的，不同的读者可能以不同的方式标记相同的系统或以不同的方式解释相同的原则。
- **不是形式分类法**：这不是一组完整或最小的设计原则。没有尝试从最小核心派生原则。

最终，这张表是一种手段，帮助学生更清楚地看到反复出现的设计原则，帮助系统设计人员更精确地传达权衡，并帮助研究人员认识到他们的想法如何融入系统设计的更广泛景观。

## 6. 结论

系统设计跨越不同的领域和词汇，这可能使共享讨论变得更加困难。我们继承机制，研究权衡，并建立直觉，但底层思想的简洁术语并不总是可用的。这里提供的设计原则"元素周期表"旨在提供一个适度的通用语言，命名反复出现的想法，使它们更容易教授、比较和构建。

## 参考文献

[1] Ron Avnur and Joseph M. Hellerstein. *Eddies: Continuously Adaptive Query Processing*. In SIGMOD, 2000.

[2] Rudolf Bayer and Edward McCreight. *Organization and Maintenance of Large Ordered Indexes*. Acta Informatica, 1972.

[3] Hal Berenson, Philip A. Bernstein, Jim Gray, Jim Melton, Elizabeth J. O'Neil, and Patrick E. O'Neil. *A Critique of ANSI SQL Isolation Levels*. In SIGMOD, 1995.

[4] Andrew D. Birrell and Bruce J. Nelson. *Implementing Remote Procedure Calls*. ACM TOCS, 1984.

[5] Craig Chambers and David Ungar. *Customization: Optimizing Compiler Technology for SELF*. In PLDI, 1989.

[6] Jeffrey S. Chase et al. *Managing Energy and Server Resources in Hosting Centers*. In SOSP, 2001.

[7] Surajit Chaudhuri and Vivek R. Narasayya. *An Efficient, Cost-Driven Index Selection Tool for Microsoft SQL Server*. In VLDB, 1997.

[8] Ron Cytron et al. *Efficiently Computing Static Single Assignment Form and the Control Dependence Graph*. ACM TOPLAS, 1991.

[9] Jeff Dean and Luiz André Barroso. *The Tail at Scale*. Communications of the ACM, 2013.

[10] Jeffrey Dean and Sanjay Ghemawat. *MapReduce: Simplified Data Processing on Large Clusters*. In OSDI, 2004.

[11] Peter J. Denning. *The Working Set Model for Program Behavior*. Communications of the ACM, 1968.

[12] Giuseppe DeCandia et al. *Dynamo: Amazon's Highly Available Key-Value Store*. In SOSP, 2007.

[13] Sally Floyd and Van Jacobson. *Random Early Detection Gateways for Congestion Avoidance*. In SIGCOMM, 1993.

[14] Goetz Graefe. *The Cascades Framework for Query Optimisation*. HPL Technical Report HPL-95-18, 1995.

[15] Jim Gray. *Why Do Computers Stop and What Can Be Done About It?* Tandem Technical Report, 1986.

[16] Jim Gray and Andreas Reuter. *Transaction Processing: Concepts and Techniques*. Morgan Kaufmann, 1993.

[17] J. N. Gray et al. *Granularity of Locks in a Shared Data Base*. In VLDB, 1975.

[18] Maurice Herlihy and J. Eliot B. Moss. *Transactional Memory: Architectural Support for Lock-Free Data Structures*. In ISCA, 1993.

[19] John Hughes. *Why Functional Programming Matters*. In *Research Topics in Functional Programming*, Addison-Wesley, 1990.

[20] Stratos Idreos et al. *Database Cracking*. In CIDR, 2007.

[21] Michael Isard et al. *Quincy: Fair Scheduling for Distributed Computing Clusters*. In SOSP, 2009.

[22] Daniel A. Jiménez and Calvin Lin. *Dynamic Branch Prediction with Perceptrons*. In HPCA, 2001.

[23] Donald E. Knuth. *Structured Programming with go to Statements*. ACM Computing Surveys, 1974.

[24] H. T. Kung and John T. Robinson. *On Optimistic Methods for Concurrency Control*. ACM TODS, 1981.

[25] Leslie Lamport. *The Part-Time Parliament*. ACM TOCS, 1998.

[26] Leslie Lamport. *The Part-Time Parliament*. ACM TOCS, 1998.

[27] Leslie Lamport. *Specifying Systems: The TLA+ Language and Tools for Hardware and Software Engineers*. Addison-Wesley, 2002.

[28] Butler W. Lampson. *Protection*. ACM Operating Systems Review, 1974.

[29] Butler W. Lampson. *Hints for Computer System Design*. ACM Operating Systems Review, 1983.

[30] Chris Lattner and Vikram Adve. *LLVM: A Compilation Framework for Lifelong Program Analysis & Transformation*. In CGO, 2004.

[31] C. L. Lawson et al. *Basic Linear Algebra Subprograms for Fortran Usage*. ACM TOMS, 1979.

[32] R. Levin et al. *Policy/Mechanism Separation in Hydra*. In SOSP, 1975.

[33] Henry Lieberman. *Your Wish is My Command: Programming by Example*. Morgan Kaufmann, 2001.

[34] Nicholas D. Matsakis and Felix Klock. *The Rust Language*. In ACM SIGAda, 2014.

[35] Robert T. Morris. *A Tour of the Worm*. USENIX, 1989.

[36] David L. Parnas. *Designing Software for Ease of Extension and Contraction*. IEEE TSE, 1979.

[37] Vern Paxson. *End-to-End Internet Packet Dynamics*. IEEE/ACM TON, 1999.

[38] K. Petersen et al. *Flexible Update Propagation for Weakly Consistent Replication*. In SOSP, 1997.

[39] Hamid Pirahesh et al. *Extensible/Rule-Based Query Rewrite Optimization in Starburst*. In SIGMOD, 1992.

[40] Gerald J. Popek and Robert P. Goldberg. *Formal Requirements for Virtualizable Third Generation Architectures*. Communications of the ACM, 1974.

[41] Dennis M. Ritchie and Ken Thompson. *The UNIX Time-Sharing System*. Communications of the ACM, 1974.

[42] J. H. Saltzer et al. *End-to-End Arguments in System Design*. ACM TOCS, 1984.

[43] Jerome H. Saltzer and Michael D. Schroeder. *The Protection of Information in Computer Systems*. Proc. IEEE, 1975.

[44] Patricia G. Selinger et al. *Access Path Selection in a Relational Database Management System*. In SIGMOD, 1979.

[45] Alexander A. Stepanov and Meng Lee. *The Standard Template Library*. HP Laboratories Technical Report, 1994.

[46] Michael Stonebraker and Lawrence A. Rowe. *The Design of POSTGRES*. In SIGMOD, 1986.

[47] R. Clint Whaley and Jack J. Dongarra. *Automatically Tuned Linear Algebra Software*. In SC, 1998.

[48] Hubert Zimmermann. *OSI Reference Model – The ISO Model of Architecture for Open Systems Interconnection*. IEEE Transactions on Communications, 1980.

## 如何引用

如果您觉得这个分析有用，请这样引用：

```
@misc{arulraj2025periodictablecomputerdesign,
      title={Towards a Periodic Table of Computer System Design Principles},
      author={Joy Arulraj},
      year={2025},
      eprint={2507.22098},
      archivePrefix={arXiv},
      primaryClass={cs.OH},
      url={https://arxiv.org/abs/2507.22098},
}
```
或

> Joy Arulraj. *Towards a Periodic Table of Computer System Design Principles* arXiv preprint arXiv:2507.22098, 2025.


## 如何贡献

我们欢迎添加、完善或重新组织原则的 PR，以及添加"经典"论文作为示例的 PR。请开一个问题描述：
  - **什么** 您正在添加或更改。
  - **为什么**（包括原则的正交性和通用性的理由）。
  - **引用**（首选经典论文）。
