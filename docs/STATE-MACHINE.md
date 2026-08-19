# V8 状态机

## 1. Booking

```text
PENDING
  ↓ confirm
CONFIRMED
  ├─ cancel → CANCELLED
  ├─ check-in → ATTENDED
  └─ expire → EXPIRED
```

候补：WAITLISTED → PROMOTED → CONFIRMED / EXPIRED。

## 2. Order

```text
CREATED
 ↓ pay
PAYING
 ↓ success
PAID
 ↓ fulfill
FULFILLED
 ├─ refund_requested → REFUNDING
 └─ refund → REFUNDED / PARTIALLY_REFUNDED
```

取消仅允许在满足业务条件时从 CREATED/未履约状态迁移。

## 3. Payment

```text
INITIATED → PROCESSING → SUCCEEDED
                    └──→ FAILED
```

支付回调必须幂等。支付结果以服务端验签后的通知为准，不信任客户端金额和状态。

## 4. Entitlement

```text
PENDING → ACTIVE
ACTIVE → RESERVED → ACTIVE
ACTIVE → CONSUMED
ACTIVE → EXPIRED
ACTIVE → FROZEN → ACTIVE
ACTIVE → REVOKED
```

数量变化必须生成 EntitlementTransaction。

## 5. Refund

```text
REQUESTED → REVIEWING → APPROVED → PROCESSING → SUCCEEDED
                         └────────→ REJECTED
```

退款成功后才真正撤销对应履约；退款处理中不能重复发起相同资源的退款。

## 6. Inventory

```text
AVAILABLE
 ↓ reserve
RESERVED
 ↓ ship
DEDUCTED
```

取消预占回到 AVAILABLE。实际库存变化通过流水完成。

## 7. Training Task

```text
LOCKED → AVAILABLE → IN_PROGRESS → COMPLETED
                      └────────────→ SKIPPED
```

已完成记录不可通过普通客户端请求回滚。

## 8. 状态机原则

- 每次迁移必须验证当前状态。
- 非法迁移返回 `INVALID_STATE_TRANSITION`。
- 状态迁移与关键流水在同一事务内完成。
- 对外事件在事务提交后发布。
- 前端只能触发动作，不能决定最终状态。
