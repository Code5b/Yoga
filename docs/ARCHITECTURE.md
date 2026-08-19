# V8 系统架构

## 1. 架构目标

第一阶段采用 **Modular Monolith（模块化单体）**，不做微服务。目标是让领域边界清晰、开发成本可控，并为未来拆分保留空间。

```text
微信小程序 / uni-app
        │ HTTPS
        ▼
┌──────────────────────────────┐
│          Spring Boot         │
│                              │
│ Identity / Member            │
│ Product / Membership         │
│ Course / Booking             │
│ Entitlement                  │
│ Order / Payment / Refund     │
│ Training / Content           │
│ Inventory                    │
│ CRM / Operation / Rule       │
│ Recommendation               │
└──────────────┬───────────────┘
               │
       ┌───────┼────────┐
       ▼       ▼        ▼
    MySQL    Redis   Object Storage
```

KMP 管理端与用户端共用 API，但权限、菜单和操作能力不同。

## 2. 模块边界

| 模块 | 职责 | 核心聚合 |
|---|---|---|
| Identity | 登录、身份、会话 | User, Session |
| Member | 会员资料、标签、生命周期 | Member, MemberProfile |
| Catalog | 可售产品定义 | Product, SKU |
| Membership | 会员产品与有效期 | Membership |
| Entitlement | 权益授予、冻结、使用 | Entitlement |
| Course | 课程与排课 | Course, Schedule |
| Booking | 预约、候补、签到 | Booking |
| Training | 训练计划、训练记录 | TrainingPlan, TrainingRecord |
| Content | 视频/数字内容版本 | Content, ContentVersion |
| Order | 统一订单 | Order, OrderItem |
| Payment | 支付与退款 | Payment, Refund |
| Inventory | 库存流水 | Inventory, InventoryTransaction |
| CRM | 会员任务与跟进 | CRMTask |
| Operation | 事件、规则、动作 | Rule, Action |
| Recommendation | 下一步推荐 | Recommendation |

## 3. 分层原则

```text
Controller
   ↓
Application Service
   ↓
Domain Service / Aggregate
   ↓
Repository
   ↓
Infrastructure
```

Controller 不直接操作数据库；业务规则不得散落在 Controller 或前端。

## 4. 事务边界

以下操作必须在单事务中完成：

- 支付成功 → 订单更新 → 权益授予；
- 签到 → 预约完成 → 权益核销；
- 取消预约 → 释放名额 → 权益返还；
- 出库 → 库存扣减 → 库存流水；
- 退款批准 → 退款记录 → 对应履约撤销/回滚。

跨事务动作通过 Outbox/Event 处理，不直接依赖同步调用链。

## 5. 非目标

V8 不引入 Kubernetes、Kafka、Elasticsearch、Service Mesh、微服务等基础设施。除非真实业务指标证明单体已经成为瓶颈，否则不提前复杂化。
