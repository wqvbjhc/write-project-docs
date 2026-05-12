---
name: write-project-docs
description: >
  为成熟项目系统化撰写完整技术文档。适用于任何类型的项目（ML/后端服务/前端/SDK/CLI/Go/C/C++/Rust/DevOps/移动端），
  覆盖从项目扫描、结构设计、内容生成到交叉验证的全流程。
  当用户提到"写文档"、"生成技术文档"、"补充项目文档"、"写 README"、
  "文档交接"、"项目文档化"、"为代码写说明"时使用此 skill。
  即使用户只是说"帮我整理下这个项目"或"新人要接手，准备下资料"，
  也应该触发此 skill。
---

# 成熟项目文档撰写方法论

适用于为已有大量代码的成熟项目补充技术文档。不限项目类型——ML 训练系统、后端服务、前端应用、SDK/库、CLI 工具、Go 服务、C/C++ 系统、Rust 项目、基础设施项目均适用。

## 核心设计目标

**让文档与代码不一致的错误在提交前被系统化地捕获。**

文档撰写最大的失败模式不是"写得不好"，而是"写的和代码不一致"。一个参数默认值写错、一条推荐命令过时、一个架构图漏画分支——比没有文档更危险。

---

## 五阶段工作流

```
Discovery → Structure → Generation → Validation → Polish
   ↑                                      |
   └──────── 用户反馈循环 ←───────────────┘
```

---

## 阶段 1: Discovery（项目扫描）

目标：建立全景理解，确定文档范围和项目类型。

### 1.1 代码结构扫描

- 目录树分析：核心模块、入口脚本、配置文件
- `git log --oneline -20` 了解近期活跃方向
- 估算代码规模（文件数、行数）

### 1.2 已有文档扫描

在动手写新文档前，先扫描项目中已有的文档资产：

- README.md、docs/ 目录、wiki 链接
- 代码注释中的 `@doc`、`///`、docstring 密度
- CHANGELOG、CONTRIBUTING、ADR（Architecture Decision Records）
- CI 中是否有文档生成步骤（rustdoc、javadoc、Sphinx、Storybook）

目的：避免重复劳动，识别可复用内容，发现已有文档与代码的不一致。

### 1.3 项目类型识别

根据目录结构和文件特征识别项目原型：

| 项目原型 | 典型目录/文件特征 |
|---------|----------------|
| ML/数据科学 | `train*.py`、`model/`、`dataset/`、`loss/`、`configs/`、`.model/.pt/.ckpt` 文件 |
| 后端服务 | `routes/`、`controllers/`、`middleware/`、`schema/`、`app.py/main.go/server.ts` |
| 前端应用 | `components/`、`pages/`、`hooks/`、`store/`、`package.json`+前端框架依赖 |
| SDK/库 | `src/`+`examples/`、`__init__.py`/`index.ts` 导出、`setup.py`/`package.json` 作为发布配置 |
| CLI 工具 | `commands/`、`bin/`、`cobra/click/yargs` 等命令框架、`man/` 页面 |
| Go 项目 | `go.mod`、`cmd/`、`internal/`、`pkg/`、`*.go` 文件 |
| 基础设施/DevOps | `terraform/`、`k8s/`、`helm/`、`Dockerfile`、`.github/workflows/`、`ansible/` |
| C/C++ 系统/应用 | `CMakeLists.txt`/`Makefile`/`meson.build`、`include/`+`src/`、`.h/.hpp/.c/.cpp` 文件、`conanfile.txt`/`vcpkg.json` |
| Rust 项目 | `Cargo.toml`、`src/main.rs`/`src/lib.rs`、`benches/`、`examples/`、`build.rs` |
| 移动端 | `ios/`、`android/`、`lib/`(Flutter)、`*.xcodeproj`、`build.gradle` |
| 全栈应用 | 前端+后端特征同时存在，通常有 `frontend/`+`backend/` 或 `client/`+`server/` |

> 一个项目可能是混合类型（如"ML 训练 + CLI 推理工具"），取主要类型，兼顾次要类型的文档需求。

### 1.4 入口点识别（启动 2-3 个 Explore agent 并行）

根据项目类型调整探索方向：

| 项目类型 | Agent A 方向 | Agent B 方向 | Agent C 方向 |
|---------|-------------|-------------|-------------|
| ML | 训练/推理入口和参数 | 数据流水线和存储 | 模型架构和部署 |
| 后端 | API 路由和中间件 | 数据模型和存储 | 认证/鉴权和部署 |
| 前端 | 页面路由和组件树 | 状态管理和数据获取 | 构建和部署配置 |
| SDK | 公开 API 和类型 | 内部实现和依赖 | 示例和测试 |
| CLI | 命令注册和参数 | 核心业务逻辑 | 配置和插件机制 |
| Go | 公开 API（pkg/）和接口 | 内部实现（internal/）| 构建/测试/部署 |
| C/C++ | 公开头文件和 API | 构建系统和依赖 | 平台适配和测试 |
| Rust | 公开 API（lib.rs）和 traits | 内部模块和依赖 | 构建/feature flags/测试 |
| DevOps | 资源定义和拓扑 | CI/CD 流水线 | 监控和告警 |

### 1.5 用户访谈（最关键的一步）

必须向用户确认：
- 推荐的使用方式（不要从代码猜测）
- 历史决策原因（为什么选 A 不选 B）
- 已知踩坑点
- 目标读者（新人接手 / 外部用户 / 自己备忘）
- 产品/业务上下文（部署在哪里、服务谁）
- **文档语言**：中文还是英文？（内部项目通常中文，开源/国际化项目用英文）

### 1.6 依赖完整性扫描

```bash
# Python 项目
grep -rh "^import \|^from " *.py | sort -u
# Node 项目
grep -rh "require(\|from '" src/ | sort -u
# Go 项目
grep -rh "\".*\"" -A0 go.mod
# C/C++ 项目
grep -rh "#include" src/ include/ | sort -u
# Rust 项目
grep -rh "^use \|extern crate" src/ | sort -u
```

### 产出

- 项目类型判定
- 已有文档资产清单（可复用/需更新/需新建）
- 模块职责清单
- 关键决策列表（含原因）
- 目标读者画像
- 文档语言决定

### 进入下一阶段的条件

- 用户确认了推荐方案和历史决策
- 已识别所有核心模块和入口
- 项目类型已确定
- 文档语言已确定

---

## 阶段 2: Structure（文档架构设计）

目标：设计文档体系的结构、分工和阅读路径。

### 按项目类型选择文档结构

| 项目类型 | 推荐文档结构 |
|---------|------------|
| ML/数据科学 | README + 快速入门 + 数据流水线 + 架构扩展 + 训练部署 + FAQ（+ SAD 架构总览） |
| 后端服务 | README + 快速入门 + API 参考 + 架构设计 + 部署运维 + FAQ |
| 前端应用 | README + 快速入门 + 组件文档 + 状态管理与数据流 + 构建部署 + FAQ |
| SDK/库 | README + 快速入门 + API 参考 + 内部架构 + 迁移指南 + 贡献指南 + FAQ |
| CLI 工具 | README + 安装使用 + 命令参考 + 配置说明 + 插件开发 + FAQ |
| Go 项目 | README + 快速入门 + API/pkg 参考 + 内部架构 + 部署运维 + 贡献指南 + FAQ |
| C/C++ 系统/库 | README + 快速入门 + API 参考 + 构建系统说明 + 架构设计 + 平台移植 + FAQ |
| Rust 项目 | README + 快速入门 + API 参考(rustdoc) + 架构设计 + Feature Flags + 贡献指南 + FAQ |
| DevOps/基础设施 | README + 快速入门 + 架构说明 + 运维手册(Runbook) + 告警与故障处理 + FAQ |
| 移动端 | README + 快速入门 + 架构设计 + 平台适配 + 构建发布 + FAQ |
| 全栈应用 | README + 快速入门 + 前端文档 + 后端文档 + 部署 + FAQ |

> 这些是起点模板。根据项目规模可增减——小项目只需 README + FAQ，大型项目可能需要 7+ 个文档。

### 设计原则（通用）

| 原则 | 做法 |
|------|------|
| 渐进深度 | 从 README 3分钟上手，到深层文档理解全部设计 |
| 单一事实源 | 一个事实只在一处详述，其他处用链接引用 |
| 双向交叉引用 | 每个文档顶部有导航栏链接相关文档 |
| 可执行示例 | 命令示例可直接复制粘贴执行 |

### README 必须包含的要素

**通用必备（所有项目类型）：**

1. **项目定位**：一句话说清楚项目做什么、基于什么
2. **快速命令**：安装 + 核心操作（训练/启动/构建/部署），各一条命令
3. **技术栈概览**：语言、框架、核心依赖的简表
4. **文档导航表**：每个子文档一行，说明覆盖什么内容

**按类型补充：**

| 项目类型 | 额外必备要素 |
|---------|------------|
| ML | 性能指标表、与基线/原版差异表、产品部署信息、历史演进时间线、文献综述 |
| 后端 | API 概览表（核心端点）、部署架构图、依赖服务列表、环境列表 |
| 前端 | 截图或 Demo 链接、浏览器/设备兼容性、设计系统/UI 框架说明 |
| SDK | 多语言/多包管理器安装方式、版本兼容矩阵、Changelog 链接、Badge |
| CLI | 核心命令速查表、配置文件示例、Shell 补全安装说明 |
| Go | godoc badge、Go 版本要求、`go install` 命令、接口设计概览 |
| C/C++ | 构建指令（cmake/make）、平台支持矩阵、ABI/API 稳定性承诺、内存管理策略说明 |
| Rust | cargo 命令速查、Feature flag 矩阵、MSRV(最低支持 Rust 版本)、unsafe 使用说明 |
| DevOps | 基础设施拓扑图、环境列表（dev/staging/prod）、访问权限与密钥管理说明 |
| 移动端 | 支持平台/版本矩阵、应用商店链接、构建变体说明 |

### 各文档的定位（按类型举例）

**ML 项目：**
- 快速入门：环境 → 数据（最小集）→ 推理 → 训练一轮 → 导出
- 数据流水线：预处理全流程 → 存储后端对比 → DataLoader 选型 → 自定义接入
- 架构扩展：整体数据流 → 模块详解 → 版本速查 → 二次开发步骤
- 训练部署：脚本选择 → 参数手册 → 命令示例 → 评估 → 导出 → 实验管理
- SAD：Mermaid 密集型架构总览（大型 ML 项目必备）

**后端服务：**
- 快速入门：环境 → 启动服务 → 调用一个 API → 查看日志
- API 参考：每个端点的 method/path/参数/响应/错误码
- 架构设计：分层架构图 → 核心模块交互 → 数据模型 → 认证流程
- 部署运维：容器化 → CI/CD → 监控 → 扩缩容 → 数据库迁移

**Go 项目：**
- 快速入门：go install/build → 运行 → 配置
- API/pkg 参考：按 package 组织，每个导出类型/函数的签名和用法
- 内部架构：cmd/ 入口 → internal/ 分层 → 依赖注入 → 错误处理策略

**SDK/库：**
- 快速入门：安装 → 最小示例 → 核心概念
- API 参考：按模块组织，每个公开方法的签名/参数/返回值/异常
- 迁移指南：版本间 breaking changes + 升级步骤

**CLI 工具：**
- 命令参考：每个命令的语法/flags/示例/注意事项
- 配置说明：配置文件格式 + 环境变量 + 优先级规则

**C/C++ 系统/库：**
- 快速入门：依赖安装 → 构建 → 运行示例/测试
- API 参考：按头文件组织，每个公开函数/类/宏的签名/参数/返回值/线程安全性
- 构建系统：CMake 选项表 / Makefile targets / 交叉编译配置
- 平台移植：支持平台矩阵、平台相关宏、条件编译说明

**Rust 项目：**
- 快速入门：cargo add → 最小示例 → 核心概念
- API 参考：与 rustdoc 互补，解释设计意图和使用模式（rustdoc 已覆盖签名细节）
- Feature Flags：每个 feature 的功能/依赖影响/启用方式

---

## 阶段 3: Generation（内容生成）

目标：生成高质量内容，每个声明有代码支撑。

### 工作流

对每个文档：
1. 启动 Explore agent 深入分析对应代码模块
2. 提取关键事实（参数/配置/签名/默认值）
3. 撰写内容
4. 内部记录每个事实的代码出处（不写入文档，后续验证用）

### 按项目类型确定事实来源

| 项目类型 | 关键事实来源 | 需 grep 验证的内容 |
|---------|------------|-------------------|
| ML | `argparse` 定义、loss/optimizer 代码、forward 方法 | 参数名、默认值、loss 权重、优化器配置 |
| 后端 | 路由注册代码、中间件链、ORM 模型定义 | 路由路径、HTTP 方法、状态码、字段类型 |
| 前端 | 组件 props/types、路由配置、store 定义 | prop 名称和类型、路由路径、action 名称 |
| SDK | 公开 API 函数签名、类型定义、导出列表 | 方法名、参数类型、返回类型、异常类型 |
| CLI | 命令注册代码、flag 定义、配置 schema | 命令名、flag 名称和简写、默认值、env var |
| Go | 导出函数/接口定义、go.mod、`cmd/` 入口 | 函数签名、接口方法、版本约束、build tags |
| C/C++ | 头文件声明、CMakeLists.txt/Makefile、`#define` 宏 | 函数签名、编译选项、宏定义值、链接库名 |
| Rust | `pub` 函数/struct/trait、Cargo.toml、feature gates | 公开 API 签名、feature flag 名、依赖版本、MSRV |
| DevOps | Terraform variables、k8s manifests、CI config | 资源名、端口号、镜像名、secret 名 |
| 移动端 | AndroidManifest、Info.plist、build.gradle/Podfile | 权限、最低版本、依赖版本 |

### 内容格式规范（通用）

| 内容类型 | 格式 |
|----------|------|
| 参数/配置说明 | 表格：名称 / 类型 / 默认值 / 说明 |
| 选型对比 | 表格：维度 / 方案A / 方案B / 推荐 |
| 架构流程 | Mermaid 图（flowchart / sequence / class） |
| 命令示例 | 代码块，含完整参数，反斜杠换行 |
| 设计决策 | 正文段落，含"原因"和"权衡" |
| API 端点 | 表格或独立小节：method / path / 参数 / 响应 |

### 命令示例规范

命令示例必须覆盖项目实际使用的语言生态，不要只写 Python 示例：

```bash
# Python/ML 项目
CUDA_VISIBLE_DEVICES="0" python train_script.py \
    --modelname ModelName \
    --dataPath /path/to/data \
    --lr 0.001

# Go 项目
go build -ldflags "-X main.version=1.2.3" ./cmd/server
./server --config /etc/app/config.yaml --port 8080

# C/C++ 项目
cmake -B build -DCMAKE_BUILD_TYPE=Release -DENABLE_TESTS=ON
cmake --build build --parallel $(nproc)

# Rust 项目
cargo build --release --features "async,tls"
cargo test --workspace --no-fail-fast

# Node/前端项目
npm run build -- --mode production
npx playwright test --project=chromium
```

规则：
- 包含必要的环境变量前缀
- 每行一个参数（反斜杠换行）
- 路径用 `/path/to/` 占位或实际路径（全文统一）
- 标注是"推荐命令"还是"历史参考"
- **参数名必须从代码中提取精确值**（不凭记忆）

### Mermaid 图表使用

- `flowchart TB/LR`：数据流、处理流程
- `sequenceDiagram`：组件交互时序
- `classDiagram`：类继承关系
- `graph TB`：模块依赖、版本演进
- `quadrantChart`：多维对比
- 用 `style` 高亮推荐节点

---

## 阶段 4: Validation（交叉验证）

目标：系统化验证正确性。这是最关键的阶段——投入应和生成阶段一样多。

### 启动验证 Agent（并行两个）

**Agent 1：代码一致性验证**

通用检查项（所有项目类型）：
- 文件名/脚本名 vs 实际存在的文件
- 环境变量名称 vs 代码中使用的名称
- 命令示例中的参数名 vs 代码中定义的参数名
- 版本号/依赖版本 vs 实际配置文件

类型特化检查项：

| 项目类型 | 必须验证的事项 |
|---------|-------------|
| ML | 参数默认值 vs argparse、loss 权重 vs 代码赋值、optimizer 配置 vs 代码、数据 shape vs forward |
| 后端 | 路由路径 vs 注册代码、HTTP 方法 vs handler、状态码 vs 返回语句、中间件顺序 vs 配置 |
| 前端 | 组件 props vs TypeScript 类型、路由路径 vs 路由配置、store action 名 vs 代码 |
| SDK | API 方法名 vs 导出、参数类型 vs 签名、返回类型 vs 实现、异常类型 vs throw |
| CLI | 命令名 vs 注册代码、flag 名 vs 定义、flag 简写 vs 代码、默认值 vs 代码 |
| Go | 导出函数名 vs 实际代码、接口方法 vs 定义、go.mod 版本 vs 实际、build tags vs 代码 |
| C/C++ | 函数签名 vs 头文件声明、CMake 选项名 vs CMakeLists.txt、宏定义值 vs `#define`、链接库名 vs target_link_libraries |
| Rust | pub API vs lib.rs 导出、feature 名 vs Cargo.toml [features]、依赖版本 vs Cargo.toml、unsafe 块是否与文档 safety 说明一致 |
| DevOps | 资源名 vs manifest、端口号 vs 配置、镜像名:tag vs Dockerfile/compose、secret 名 vs 引用 |

**Agent 2：完整性与一致性验证**

- 同一事实在不同文档中描述是否一致
- 交叉引用链接是否有效（`ls` 验证目标文件存在）
- 推荐方案是否全文统一
- 每个公开脚本/命令/API 是否都有使用说明
- 依赖文件（requirements.txt / package.json / go.mod / Cargo.toml）是否包含所有实际 import

### 常见错误模式

| 错误模式 | 示例 | 预防方法 |
|----------|------|----------|
| 参数/flag 名写错 | `--model` 写成 `--modelname`；`-p` 写成 `--port` | grep 代码中的参数定义语句 |
| 默认值/配置值写错 | Loss 权重/端口号/超时时间写成旧版本 | grep 代码中的精确赋值语句 |
| 命令示例不可执行 | 参数组合不兼容、路径不存在 | 交叉验证参数间的依赖关系 |
| 路由/端点路径写错 | `/api/user` 写成 `/api/users` | grep 路由注册代码 |
| A/B 说反 | "有 X 字样的是 Y 版本" | 从文件名和代码双重确认 |
| 遗漏分支 | 数据流图少画一个输出/API 漏列一个端点 | 对照 return/export/注册代码 |
| 依赖遗漏 | 实际必需的包未列入依赖文件 | grep import 全量扫描 |
| 文件名 typo | 项目结构列表写错文件名 | ls 实际目录对比 |
| 交叉引用断链 | 文件改名后引用未更新 | ls 验证所有 `[xxx](file.md)` 中 file.md 存在 |
| FAQ 通用化 | 解决方案用通用模板而非项目实际代码 | FAQ 必须引用项目真实文件路径和函数名 |
| 数值跨文档不一致 | 版本号/端口/opset 在不同文档说法不同 | 一个数值只在一处定义，其他处链接引用 |
| 环境变量名不一致 | 文档写 `DB_URL`，代码用 `DATABASE_URL` | grep 代码中 `os.getenv/process.env/os.Getenv` |
| Docker 镜像 tag 过时 | 文档写 `v1.2`，实际已更新到 `v2.0` | 查看 Dockerfile/compose 中的实际 tag |
| API 响应格式过时 | 文档中的 JSON 响应与实际 handler 返回不一致 | 对照 handler 的 return/response 语句 |

### 用户确认轮

将验证发现汇总给用户，重点关注：
- 发现的事实性错误（逐条列出）
- 推荐方案是否正确
- 是否有遗漏的重要信息

---

## 阶段 5: Polish（最终润色）

### 操作

1. **修正验证阶段发现的所有问题**
2. **格式统一**：表格对齐、代码块语言标注、标题层级一致
3. **依赖文件更新**：补全缺失包
4. **提交**：清晰的 commit message 列出所有变更

### 提交前最终检查

- [ ] 每个命令示例语法正确，参数名从代码定义中验证
- [ ] 每个表格数值从代码验证过
- [ ] Mermaid 图表无语法错误
- [ ] 文档导航链接全部有效（ls 验证目标文件存在）
- [ ] 同一数值/配置在所有文档中一致
- [ ] FAQ 中的代码示例引用项目真实文件名和函数名
- [ ] 依赖文件完整（requirements.txt / package.json / go.mod / Cargo.toml）
- [ ] .gitignore 不排斥 docs 目录
- [ ] README 包含：项目定位、快速命令、技术栈、文档导航 + 类型特有要素

---

## 反模式

1. **凭记忆写参数/flag/函数签名** — 永远从代码中 grep 精确值，不凭印象
2. **编造 FAQ** — 只记录真实遇到过的问题（问用户）
3. **一次性写完不验证** — 必须有独立的交叉验证环节
4. **所有信息塞进 README** — 分层分文档，README 只做入口
5. **假设推荐方案** — 必须问用户确认，代码中的"默认"不等于"推荐"
6. **多处重复同一事实** — 一处详述 + 其他处链接引用
7. **忽略历史上下文** — "为什么这样选"比"选了什么"更有价值
8. **FAQ 用通用代码模板** — FAQ 的代码示例必须引用项目实际文件名、函数名、变量名
9. **交叉引用链接不验证** — 文档内的 `[链接](file.md)` 必须与实际文件名一致
10. **同一事实多处不一致** — 如版本号在不同文档说法不同，必须统一到一处权威说明
11. **忽视项目类型差异** — 不同类型项目的文档侧重点、验证方法、命令格式完全不同，不能用同一套模板硬套
12. **文档写完就不管** — 文档是活的，代码变了文档也要跟着变（见下方"文档维护"）

---

## 适配不同项目规模

| 规模 | 文档量 | 重点 |
|------|--------|------|
| 小型（<5k 行） | README + FAQ | 快速上手、已知限制 |
| 中型（5k-50k 行） | README + 2-3 个专题 | 核心流程、选型决策 |
| 大型（>50k 行） | 完整 5-7 个文档 | 全部阶段、架构总览图 |

## 适配不同项目类型

| 项目类型 | 文档最大风险 | 最重要的验证点 | 图表侧重 |
|---------|------------|-------------|---------|
| ML | 训练参数与代码不一致 | argparse 默认值、loss 配置 | 数据流图、模型结构图 |
| 后端 | API 文档与实际行为不一致 | 路由路径、请求/响应格式 | 架构分层图、时序图 |
| 前端 | 组件用法与 props 不一致 | TypeScript 类型、路由配置 | 组件树、状态流图 |
| SDK | API 签名与实现不一致 | 公开方法签名、类型定义 | 类图、调用关系图 |
| CLI | 命令/flag 文档与代码不一致 | 命令注册、flag 定义 | 命令层级图、配置优先级图 |
| Go | 导出 API 与 godoc 不一致 | 接口签名、go.mod 版本 | 包依赖图、请求流程图 |
| C/C++ | 头文件声明与实现/构建不一致 | 函数签名 vs 头文件、CMake 选项 vs 代码 | 模块依赖图、内存生命周期图 |
| Rust | API/feature 文档与 Cargo.toml 不一致 | pub API vs 导出、feature flags vs Cargo.toml | trait 关系图、模块层级图 |
| DevOps | 部署文档与实际配置不一致 | manifest 中的资源名/端口/镜像 | 基础设施拓扑图、CI/CD 流水线图 |

---

## 文档维护

文档写完不是终点。为项目建立文档保鲜机制：

1. **在 PR review 中检查文档影响**：修改了参数/API/配置时，对应文档是否同步更新
2. **建议在 CI 中加入检查**：如 `docs/` 目录的 link checker、Mermaid 语法检查
3. **版本号集中管理**：将频繁变化的数值（版本号、端口、镜像 tag）集中在一处，其他文档引用
4. **定期审查触发器**：建议用户在大版本发布、重构完成、新人入职时重新验证文档准确性
