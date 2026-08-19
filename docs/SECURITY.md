# V8 安全、权限与幂等

## 1. 身份

用户端使用微信登录建立内部 User；管理端使用独立管理员身份体系。外部身份只作为 identity mapping，不作为业务主键。

## 2. RBAC

第一阶段角色：

- OWNER：门店经营者
- ADMIN：店长/管理员
- COACH：教练
- FRONT_DESK：前台
- OPERATOR：运营

权限按资源 + 动作控制，例如：`member.read`、`member.write`、`order.refund`、`inventory.adjust`。

## 3. 数据安全

- 密码/Token 不进入日志；
- 微信身份敏感字段最小化保存；
- 日志不得打印完整支付信息；
- 管理端关键操作记录 audit log；
- 导出会员数据必须记录操作者和范围。

## 4. 幂等

支付、预约、签到、权益核销、退款、库存扣减、事件消费全部必须支持幂等。

幂等键生命周期至少覆盖一次业务重试窗口；重复请求返回首次业务结果，而不是重复执行。

## 5. 并发

座位、库存、权益余额属于竞争资源，使用数据库条件更新/锁或版本号控制并发。典型操作：

```sql
UPDATE entitlements
SET available_quantity = available_quantity - 1
WHERE id = ? AND available_quantity >= 1;
```

影响行数为 0 时判定资源不足或状态变化，不能继续履约。

## 6. 审计

至少审计：退款、库存调整、会员权益人工调整、订单取消、管理员权限变化、会员敏感信息导出。

Audit 记录：actor、action、resource、resourceId、before、after、requestId、createdAt。

## 7. 防重与频控

运营任务、优惠、推荐消息需要业务级频控，例如同一会员同一规则 7 天内最多创建一次任务，避免骚扰用户和员工。
