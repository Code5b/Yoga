# V8 数据库设计

数据库：MySQL 8.x。所有业务表使用 bigint 主键、UTC/门店时区明确的 datetime、created_at/updated_at。金额使用最小货币单位整数，不使用浮点数。

## 1. 核心表

### Identity / Member

- users
- user_identities
- members
- member_tags
- member_tag_relations

### Catalog / Membership

- products
- product_items
- skus
- memberships
- membership_products

### Entitlement

- entitlements
- entitlement_transactions

`entitlement_transactions` 是权益数量变化的事实流水，类型包括 GRANT、RESERVE、CONSUME、RELEASE、EXPIRE、ADJUST、REFUND。

### Course / Booking

- courses
- course_schedules
- bookings
- booking_waitlists
- attendances

### Training / Content

- training_plans
- training_plan_versions
- training_tasks
- training_records
- contents
- content_versions
- content_access
- playback_progress

### Order / Payment

- orders
- order_items
- payments
- payment_transactions
- refunds
- refund_allocations

### Inventory

- inventory
- inventory_transactions
- stock_reservations

### CRM / Operation

- events
- event_outbox
- rules
- rule_executions
- crm_tasks
- recommendations
- recommendation_actions

## 2. 关键约束

### 幂等

以下业务必须有业务幂等键：

- payment transaction
- booking create/cancel
- entitlement consume/release
- inventory deduction
- refund
- event processing

推荐字段：`idempotency_key` + `unique(member_id, idempotency_key)` 或按业务主体建立唯一约束。

### 金额

```text
amount_minor BIGINT
currency CHAR(3)
```

禁止 Double/Float 存金额。

### 状态与流水

当前状态用于快速查询，流水用于审计和恢复。不要只保存最终状态。

## 3. 索引原则

高频查询优先建立：

- members(status, created_at)
- bookings(schedule_id, status)
- bookings(member_id, start_at)
- entitlements(member_id, type, status, valid_to)
- orders(member_id, created_at)
- orders(status, created_at)
- crm_tasks(status, assignee_id, due_at)
- events(type, occurred_at)

不要为所有字段机械建索引。

## 4. 退款拆分

套餐退款必须通过 `refund_allocations` 把退款金额映射到具体 OrderItem，并结合：

- 已使用次数；
- 数字内容访问/完成情况；
- 商品发货状态；
- 优惠分摊；
- 已授予权益；

计算可退款金额。退款不能简单按订单总额比例倒推。

## 5. 数据一致性

数据库唯一约束负责最终一致性底线；应用层负责业务校验；Outbox 负责事务内记录待发送事件，避免“数据库成功但消息丢失”。
