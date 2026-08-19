# V8 API 规范

## 1. 基础约定

Base Path：`/api/v1`

JSON UTF-8；时间使用 ISO-8601；金额使用最小货币单位；所有写操作返回业务资源或明确的 operation id。

统一响应：

```json
{
  "code": "SUCCESS",
  "message": "ok",
  "data": {},
  "requestId": "..."
}
```

错误响应必须包含 `code`、`message`、`requestId`。

## 2. 认证

```text
POST /auth/wechat/login
POST /auth/refresh
POST /auth/logout
```

Access Token 短时有效；Refresh Token 可撤销。

## 3. 会员

```text
GET  /members/me
GET  /members/{id}
PATCH /members/{id}
GET  /members/{id}/entitlements
```

## 4. 课程与预约

```text
GET  /courses
GET  /schedules
POST /bookings
GET  /bookings/{id}
POST /bookings/{id}/cancel
POST /bookings/{id}/check-in
```

预约创建、取消、签到必须支持幂等。

## 5. 商品与订单

```text
GET  /products
GET  /products/{id}
POST /orders
GET  /orders/{id}
POST /orders/{id}/cancel
POST /payments
POST /refunds
```

订单创建不代表权益已生效；必须以支付确认后的履约事务为准。

## 6. 训练

```text
GET  /training/plans
GET  /training/plans/{id}
GET  /training/plans/{id}/tasks
POST /training/tasks/{id}/complete
GET  /training/records
```

训练完成接口必须幂等，并保留完成事实。

## 7. 管理端

```text
GET  /admin/dashboard/today
GET  /admin/tasks
POST /admin/tasks/{id}/complete
GET  /admin/members
GET  /admin/orders
GET  /admin/inventory
GET  /admin/analytics/overview
```

管理端 API 必须进行角色权限检查，不能仅依赖前端隐藏菜单。

## 8. 幂等

客户端可发送：

```text
Idempotency-Key: <uuid>
```

服务端保存请求结果和资源关联。相同 key + 相同业务主体重复请求返回原结果；相同 key 不同请求体返回 `IDEMPOTENCY_CONFLICT`。

## 9. 分页

统一使用：

```text
page
pageSize
```

返回：

```json
{
  "items": [],
  "page": 1,
  "pageSize": 20,
  "total": 100
}
```

## 10. API 设计原则

API 暴露业务动作，而不是数据库 CRUD。例如使用 `/bookings/{id}/cancel`，不要让客户端直接 PATCH status。状态迁移由领域服务控制。
