# 4+1 视图 PlantUML 模板

openFuyao 云原生项目方案设计中 4+1 架构视图的 PlantUML 代码模板。
示例以 K8s 云原生场景为背景，根据实际特性替换参与者、组件、流程。

---

## 用例视图（+1 视图）

展示核心参与者和使用场景，回答"系统为谁做什么"。

```plantuml
@startuml use-case-view
left to right direction
skinparam packageStyle rectangle

actor "集群管理员" as admin
actor "应用开发者" as dev
actor "K8s API Server" as apiserver <<system>>
actor "CI/CD Pipeline" as cicd <<system>>

rectangle "<特性名称>" {
  usecase "UC1: 创建自定义资源" as UC1
  usecase "UC2: 查看资源状态" as UC2
  usecase "UC3: 配置弹性伸缩策略" as UC3
  usecase "UC4: 触发滚动升级" as UC4
}

admin --> UC1
admin --> UC3
dev --> UC2
dev --> UC4
cicd --> UC4
UC1 ..> apiserver : <<delegate>>
UC3 ..> apiserver : <<delegate>>
@enduml
```

**编写要点**：

- 参与者区分人类角色与系统角色（用 `<<system>>` 标记）
- 用例命名：动词 + 对象（如"创建自定义资源""配置弹性伸缩策略"）
- 系统边界用 `rectangle` 框定特性范围
- K8s 生态中常见系统角色：API Server、Controller Manager、Scheduler、Kubelet

---

## 逻辑视图

展示模块/包的职责划分与依赖关系，回答"系统由哪些逻辑单元组成"。

```plantuml
@startuml logical-view
skinparam componentStyle uml2
skinparam package {
  BackgroundColor #F8F9FA
  BorderColor #DEE2E6
}

package "API 层" {
  [CRD 定义 & Webhook] as api
}

package "控制面" {
  [Reconciler / Controller] as ctrl
  [状态机 / 策略引擎] as engine
}

package "数据面" {
  [Agent / DaemonSet] as agent
  [运行时接口 (CRI/CNI/CSI)] as runtime
}

package "基础设施" {
  [K8s Client (client-go)] as k8sclient
  [配置管理 (ConfigMap/Secret)] as config
  [可观测性 (Metrics/Logs/Traces)] as observability
}

api --> ctrl : Watch & Enqueue
ctrl --> engine : 决策
ctrl --> k8sclient : CRUD 资源
engine --> agent : 下发指令
agent --> runtime : 调用运行时
ctrl ..> config : 读取配置
ctrl ..> observability : 上报状态
@enduml
```

**编写要点**：

- 按 K8s 惯例分为 API 层 / 控制面 / 数据面 / 基础设施
- 标注依赖方向（实线=强依赖）和可选依赖（虚线）
- 新增模块用不同背景色标注：`BackgroundColor #E3F2FD`
- 与项目实际包结构对齐（参考项目的架构文档或 README）

---

## 开发视图

展示代码组织、分层与构建单元，回答"代码如何组织"。

```plantuml
@startuml development-view
skinparam packageStyle frame

package "项目根目录 (Go Module)" {

  package "api/v1beta1/" <<folder>> {
    [types.go] as types
    [zz_generated.deepcopy.go]
    [webhook.go] as webhook
  }

  package "internal/controller/" <<folder>> {
    [<resource>_controller.go] as ctrl
    [<resource>_controller_test.go] as ctrl_test
  }

  package "pkg/" <<folder>> {
    [<业务逻辑>.go] as biz
    [client/] as client
  }

  package "config/" <<folder>> {
    [CRD YAML] as crd_yaml
    [RBAC YAML] as rbac
    [Webhook 证书] as cert
  }

  package "cmd/" <<folder>> {
    [main.go] as main
  }
}

package "外部依赖" <<cloud>> {
  [controller-runtime] as cr
  [client-go] as clientgo
  [K8s API machinery] as apimachinery
}

main --> ctrl
ctrl --> types : 引用 CRD 类型
ctrl --> biz : 业务逻辑
ctrl --> cr : Reconciler 框架
biz --> client : K8s / 外部服务调用
client --> clientgo
types --> apimachinery
ctrl_test --> ctrl

note right of crd_yaml
  make manifests 生成
  kustomize 组装部署
end note
@enduml
```

**编写要点**：

- K8s Operator 项目常见分层：`api/` → `internal/controller/` → `pkg/` → `cmd/`
- 区分内部包和外部模块（`<<cloud>>` 标记外部）
- 代码生成产物（`zz_generated.`*）标注来源
- 新增文件/包用注释标注 `<<new>>`

---

## 运行视图

展示关键场景的运行时交互时序，回答"组件间如何协作"。

### 模板 A：时序图（适用于 Reconcile 调用链）

```plantuml
@startuml runtime-sequence
skinparam sequenceArrowThickness 1.5
skinparam sequenceParticipantPadding 20

actor "用户 / CI" as user
participant "kubectl / API" as api
participant "Controller\nManager" as cm
participant "Reconciler" as reconciler
participant "K8s API Server" as apiserver
participant "目标组件\n(Pod/Service/...)" as target

user -> api : kubectl apply -f cr.yaml
activate api
api -> apiserver : Create/Update CR
activate apiserver
apiserver --> api : 201 Created
deactivate api

apiserver -> cm : Watch Event
activate cm
cm -> reconciler : Reconcile(req)
activate reconciler

reconciler -> apiserver : Get CR
apiserver --> reconciler : CR spec

reconciler -> apiserver : Create/Update 子资源
activate apiserver
apiserver -> target : 调度 & 创建
apiserver --> reconciler : ok
deactivate apiserver

reconciler -> apiserver : Update CR Status
reconciler --> cm : Result{RequeueAfter: 30s}
deactivate reconciler
deactivate cm
@enduml
```

### 模板 B：活动图（适用于流程编排）

```plantuml
@startuml runtime-activity
start
:接收 Reconcile 请求;
:从 API Server 获取 CR;

if (CR 已被删除?) then (是)
  :执行 Finalizer 清理;
  :移除 Finalizer;
  stop
endif

:校验 Spec 字段;

if (校验通过?) then (是)
  :计算期望状态 (Desired State);
else (否)
  :更新 Status 为 Invalid;
  :记录 Warning Event;
  stop
endif

:读取当前状态 (Current State);

if (当前 == 期望?) then (是)
  :更新 Status 为 Ready;
else (否)
  :执行变更 (Create/Update/Delete 子资源);
  if (变更成功?) then (是)
    :更新 Status 为 Progressing;
    :设置 RequeueAfter;
  else (否)
    :更新 Status 为 Error;
    :记录 Error Event;
  endif
endif

stop
@enduml
```

**编写要点**：

- 每个关键场景（SR 中的 Must 级功能）至少一个时序图
- 标注 `activate`/`deactivate` 显示生命周期
- 错误路径用 `alt`/`else` 分支或活动图 `if` 展示
- K8s 特有模式：Reconcile 循环、Finalizer 清理、Status 更新、Event 记录
- 循环等待/重试用 `loop` 片段

---

## 部署视图

展示物理/逻辑节点、容器与网络拓扑，回答"系统如何部署"。

```plantuml
@startuml deployment-view
skinparam nodeBackgroundColor #F8F9FA
skinparam artifactBackgroundColor #E3F2FD

node "管理集群 (Management Cluster)" as mgmt {
  artifact "API Server" as apiserver
  artifact "etcd" as etcd
  artifact "Controller Manager" as cm {
    artifact "<特性> Controller" as feature_ctrl
  }
  artifact "Webhook Server" as webhook
}

node "工作负载集群 (Workload Cluster)" as workload {
  node "Control Plane Node" as cp {
    artifact "API Server" as w_api
    artifact "etcd" as w_etcd
    artifact "Scheduler" as w_sched
  }
  node "Worker Node × N" as worker {
    artifact "kubelet" as kubelet
    artifact "容器运行时 (containerd)" as crt
    artifact "应用 Pod" as pod
  }
}

database "镜像仓库\n(Harbor / Registry)" as registry
artifact "Helm Chart 仓库" as helm

mgmt --> workload : 生命周期管理
feature_ctrl --> apiserver : Watch/CRUD
cm --> webhook : 准入校验
worker --> registry : 拉取镜像
kubelet --> crt : CRI 调用

cloud "外部网络 (可选)" <<optional>> {
  artifact "公有云 API" as cloud_api
  artifact "远程镜像源" as remote_reg
}

registry <..> remote_reg : 镜像同步 (离线场景)
feature_ctrl <..> cloud_api : 基础设施操作 (可选)
@enduml
```

**编写要点**：

- `node` 对应物理/虚拟主机，`artifact` 对应进程/容器/服务
- K8s 典型拓扑：管理集群 + 工作负载集群，或单集群自管理
- 离线场景下外部网络用虚线 + `<<optional>>` 标注
- 多架构部署标注 `amd64`/`arm64`（如 `node "Worker (arm64)"`）
- 持久化存储标注 `database` 类型

---

## 综合提示

1. **命名一致性**：PlantUML 中的组件名与代码包名、设计文档中的模块名保持一致
2. **颜色语义**：新增组件 `#E3F2FD`（蓝）、修改组件 `#FFF3E0`（橙）、删除组件 `#FFEBEE`（红）
3. **复杂度控制**：单个图不超过 15 个组件/参与者，超出时拆分为子图
4. **语法校验**：输出后建议在 [PlantUML 在线编辑器](https://www.plantuml.com/plantuml/uml) 或 IDE 插件中渲染确认，避免语法错误
5. **MCP 渲染**：实际生成文档时，优先使用 PlantUML MCP 生成 SVG 图片并保存到 `figures/` 目录，详见 [SKILL.md 块 2 的 PlantUML 渲染与存储规范](SKILL.md#块-2-4+1-架构视图)
